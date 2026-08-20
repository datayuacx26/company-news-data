---
schema_version: "1.0.0"
document_id: "479c9622e7347f98ba172089fe5cf94e34010b3c33160928a73e6c1012309228"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/learn-redis-the-hard-way/"
published_at: "2017-01-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:28.827282+00:00"
content_hash: "sha256:2e5875893e641dfdb6420ca5af8040cdae40b9e576c96386a0f92229fc0050b2"
---

# Learn Redis the hard way (in production)

For our products, like the trivago[hotel search](https://www.trivago.com/) , we are using[Redis](http://redis.io/) a lot. The use cases vary: Caching, temporary storage of data before moving those into another storage or a typical database for hotel meta data including persistence.


The main parts of the hotel search are built with PHP and the Symfony Framework for the frontend (web) and Java for the backend part. In this article, we will focus on the collaboration between our PHP application and Redis. Both are running fine, but it was a long and hard way up to the current situation. This is the story of how we learned to use Redis, including our failures and experience.


## The beginning


This story began on Friday, September 3, 2010. At 10:33am the first classes of[Predis](https://github.com/nrk/predis) , a Redis client library for PHP, were committed into our codebase.


This moment can be marked as the introduction of Redis into our PHP stack. Fast forward to February 2013. We replaced the library Predis (PHP implementation) with the PHP extension[phpredis](https://github.com/phpredis/phpredis) (C implementation). The reason was simple: Performance. The replacement went well. Everything with Redis was fine and we enjoyed the summer that year.


The real fun started exactly one year later in February 2014. Around this time, we launched new features and added new languages to our platform. The result of this: **The HTTP traffic doubled in a short time** . Due to good capacity planning on the hardware side, we were able to handle the growth. On the software side however, we were confronted with 40% of the incoming requests resulting in **HTTP 500: Internal Server Error** .


After investigating our logs, we saw errors related to the PHP - Redis connection handling. Most of them were[read error on connection](https://github.com/phpredis/phpredis/blob/1fa240478ff5c1be4dd769c759859b7f66db3526/library.c#L443) and[Redis server went away](https://github.com/phpredis/phpredis/blob/607988ad8dcb6dbaac1b3af7329042c7da62212d/redis.c#L447) . Our logging was verbose and gave us enough detail to start our debugging session:


```text
www7.trivago.com   20140202102259941   |   WARN   |   ...   Redis  \C  onnectException:   Unable   to   connect:   read   error   on   connection   ...
#0 /.../vendor/.../Redis/RedisPool.php(106): ...\Redis\RedisPool->connect(Object(Redis), Object(...\Redis\RedisServerConfiguration))
#1 /.../vendor/.../Redis/RedisClient.php(130): ...\Redis\RedisPool->get('default', true)
#2 /.../vendor/.../Redis/RedisClient.php(94): ...\Redis\RedisClient->setMode(false)
...
#17 /.../app/bootstrap.php.cache(551): Symfony\Bundle\FrameworkBundle\HttpKernel->handle(Object(Symfony\Component\HttpFoundation\Request), 1, true)
#18 /.../web/app.php(15): Symfony\Component\HttpKernel\Kernel->handle(Object(Symfony\Component\HttpFoundation\Request))
#19 {main}
|   12.34.56.78   |   www.trivago.de   |   /?aDateRange%5Barr%5D  =2014-05-20&  aDateRange%5Bdep%5D  =2014-05-21&iRoomType  =  1  &iPathId  =  44742  ...   |   Mozilla/5.0   (WindowsNT   6.1  ;   Trident/7.0  ;   rv:11.0  ) like Gecko
```


A quick google search showed that we were not alone with this issue. See[debug read error on connection #70](https://github.com/phpredis/phpredis/issues/70) and[‘read error on connection’ #492](https://github.com/phpredis/phpredis/issues/492) .


## Debugging and fixing the issue


Based on the discussion in the tickets, we thought: *Congratulations, we got ourselves a nasty bug there* .


We had no clue what the root cause was. Redis had been working fine for more than 3.5 years, and had never caused us any trouble before. Leaving us with the question: How do we continue from here? Our first attempt was to try everything that was mentioned in the Github issue:


- Raising PHP connection and command timeouts from 500ms to 2.5 seconds
- Disabling the PHP setting` default_socket_timeout`
- Disabling SYN cookies on the host systems
- Checking the number of file descriptors on Redis and Webservers
- Raising the[mbuffer](https://www.freebsd.org/cgi/man.cgi?query=mbuffer&sektion=1&apropos=0&manpath=FreeBSD+9.0-RELEASE+and+Ports) of the host systems
- Control and adjust the TCP backlog sizes
- and much more


Nothing helped to solve this issue in a reliable way. So back to traditional debugging! We tried to reproduce the issue in our pre-production environments. Sadly, we were not successful. We thought that those issues only appeared with much higher traffic. So we continued to deep dive into our applications …


### Closing Redis connections at the end of a web request


PHP applications are usually stateless. Everything you allocate in a request is gone once the request finishes. At this time, we were not using php-fpm and persistent connections. This means that every HTTP request would create a new Redis connection. While checking our connection handling to Redis, we opened a connection, but never closed it.


This should not make a difference in newer PHP versions. PHP will automatically close the connection when your script ends. In older versions, however, this could lead to problems like stale connections or memory leaks. **Besides that: It is good practice to close your connections** , so we fixed it. But that did not help us with our initial problem.


### A/B-Testing of connection libraries


We kept on searching and asked ourselves if we hit a bug in phpredis (php extension). To verify this hypothesis, we implemented an A/B-Test. The necessary infrastructure to run A/B-Tests was already there. So we used it and switched from the C extension back to the` predis` library.


We were pleased to find that` predis` was still being maintained and had received a lot of development love.


Due to good code structure, this change was done quickly. We implemented one interface, replaced the phpredis connection implementation with predis and reconfigured our dependency injection container.


We deployed the test to 20% of our users in one datacenter. The errors occured again. In both libraries! Was this another failure on our road to fix this bug? No! We considered it a partial success. We were able to exclude the extension (` phpredis` ) as a possible root cause.


### Upgrade Redis


Our next step was to have a deeper look at the Redis side. Common steps in an investigation of a bug are checking out the issue tracker of the project or getting in contact with one of the maintainers. The first thing they will ask is “ *What version are you running?* “. Once you mention a version that is not the latest in the upstream, they will answer “ *Please upgrade and check if it still occurs* ”. This is what we did.


At this time we were running Redis v2.6. The latest upstream version was v2.8.9. We thought that maybe we hit a bug that was already resolved. Unfortunately, this was not the case. We made no progress related to our problem, but at least our Redis servers were up to date. :)


### Debugging latency problems


After reading a lot of documentation, we came across a feature for debugging latency problems. It’s called[Redis Software Watchdog](http://redis.io/topics/latency#redis-software-watchdog) . It was (and still is) marked as *experimental* in the official documentation but we wanted to give it a try. The idea was to identify long running and blocking commands.


> **Tip** >[Redis is single threaded](http://redis.io/topics/latency#single-threaded-nature-of-redis) . Every command may block other commands. Keep this in mind if you start thinking about problems or use cases. This seems obvious, but is mostly overlooked and the root cause of many issues.


So we activated Watchdog, waited a few seconds and **[Murphy’s law](https://en.wikipedia.org/wiki/Murphy's_law) kicked in** . We hit a bug and our Redis server crashed. In production! See[Software watchdog crashes redis during rdb save point #1771](https://github.com/antirez/redis/issues/1771) . Again: Related to our main problem, we made no significant progress at this time. Instead, we had a new problem: An offline Redis database (which was immediately restarted, of course). So we kept going.


We started the next try by measuring the[latency baseline](http://redis.io/topics/latency#latency-baseline) of our Redis setup. The numbers of the *intrinsic-latency* looked pretty good. The base latency looked horrifying.


```text
$   redis-cli   --latency   -p   6380   -h   1.2  .3.4
min:   0  ,   max:   463  ,   avg:   2.03   (19443   samples  )
```


We checked the Redis logs and discovered that Redis was saving data to disk every few minutes:


```text
...
[  20398  ] 22 May 09:20:55.351   *   10000 changes   in   60 seconds. Saving...
[  20398  ] 22 May 09:20:55.759   *   Background saving started by pid 41941
[  41941  ] 22 May 09:22:48.197   *   DB saved on disk
[  20398  ] 22 May 09:22:49.321   *   Background saving terminated with success
[  20398  ] 22 May 09:25:23.299   *   10000 changes   in   60 seconds. Saving...
[  20398  ] 22 May 09:25:23.644   *   Background saving started by pid 42027
[  20398  ] 22 May 09:26:50.646   # Accepting client connection: accept: Software caused connection abort
[  20398  ] 22 May 09:26:50.900   # Accepting client connection: accept: Software caused connection abort
...
```


We run Redis on bare metal servers with (nearly) default configuration, because it is shipped with sane defaults. Our first question was *Why did the fork of a background saving process take ~400ms?* (have a look at the first two log lines). After reading a few mailinglist posts and the implementation of[BGSAVE](https://github.com/antirez/redis/blob/e9d861ec69a11208578fc2a8b7dcdf4c52df316e/src/rdb.c#L1878) , we understood why. Redis is forking a background process and needs to copy the[page table](https://en.wikipedia.org/wiki/Page_table) . So, if you have a big Redis instance with many keys, it will take time. Even on bare metal, without virtualization. By now, this behaviour has been added to the offical documentation. See[Fork time in different systems](http://redis.io/topics/latency#fork-time-in-different-systems) .


As a follow-up we deactivated[Redis snapshots](http://redis.io/topics/persistence#snapshotting) for services where persistence was not needed. This reduced the amount of` read error on connection` by more than 30%.


For instances where persistence is needed, the usage of snapshot points can be tricky. If you have a lot of traffic on your instances and your application is doing write operations per request you will have more key modifications. This leads to more` BGSAVE` triggers and (possibly) a bigger amount of rejected connections. The reasons are higher process fork times and blocked Redis instances.


> **Tip** Review your persistence requirements and Redis configuration. Does your app modify more keys when you get more traffic and do you need persistence? Consider[AOF](http://redis.io/topics/persistence#aof-advantages) or rolling` BGSAVE` as an alternative to standard snapshotting. This may avoid connection/command execution timeouts and blocking Redis instances.


This was the case in our situation. Our application is reading and writing keys, but we didn’t want to deactivate persistence globally. We deactivated the snapshot points in those Redis instances and activated cronjobs that will call the` BGSAVE` command at a specific time (rolling BGSAVE). With this we know when a dump is triggered and can avoid those during high traffic times. An alternative to rolling BGSAVE operations would be a separate slave instance for persistence. This slave will not handle real traffic and its only purpose is to take care of persistence. In use cases with higher persistence requirements we prefer to use[AOF](http://redis.io/topics/persistence#aof-advantages) . If you want to know more about “rolling BGSAVE” we started a small post on the old Redis mailinglist to discuss this topic. See[Rolling BGSAVE instead of (pre)-configured save points](https://groups.google.com/forum/#!topic/redis-db/xJRJkOIyW_g) .


This change was **considered a success** . We reduced our error/timeout problems immediately (even in non-peak traffic times). But we still saw errors popping up here and there, so we were not done yet.


### One instance per data context


Since Redis has been introduced into our web stack, it was adopted by more and more teams for various use cases.


They used the existing Redis instances and stored their data in a[different database](http://redis.io/commands/select) . This way, they could start right a way. This was great for several reasons, but led us to our next challenge.


One team had a cronjob running every 15 minutes that dumped data from a MySQL database into a shared Redis instance via the[Pipelining](http://redis.io/topics/pipelining) feature. Due to the single threaded nature of Redis the shared Redis instance was blocked every 15 minutes for several seconds.


> **Tip** Be aware of Redis instances that are shared between teams and data context. Due to different use cases and long running commands they may block each other. Remember: Redis is single threaded.


We moved the cronjob to its own Redis instance. As a result, the (former) shared instance threw a lot less connection and timeout errors than before. Due to the split of data contexts the amount of commands per instance was reduced. Furthermore, starting several Redis instances per server leads to a better utilization of computing resources. Why? Again: **Redis is single threaded!** And modern servers have a lot of cores.


*Side note* : The usage of[SELECT](http://redis.io/commands/select) and multiple databases inside one Redis instance was mentioned as an[anti pattern](https://groups.google.com/forum/#!topic/redis-db/vS5wX8X4Cjg) by[Salvatore](https://twitter.com/antirez) .


### O(n) can kill you


Okay, we found several causes and reduced the amount of connection and timeout errors by an order of magnitude. Everything went well for a long time and our Redis setup was healthy. The time went by, teams implemented new features into our application and our traffic continued to grow. The traffic growth went fast and the connection and command timeout errors came back.


Our first thought: *Really? Murphy? Are you there?*


Luckily we saw a pattern in the occurrence of the message. It occurred periodically every 5 minutes. Based on our knowledge from the last investigation we started right away. We measured the base latency, enabled watchdog and read the[SLOWLOG](http://redis.io/commands/slowlog) documentation.


In a very short time, compared to previous investigations, we identified a cronjob that fires the[KEYS *](http://redis.io/commands/keys) command against a big Redis instance. Luckily the Redis documentation describes the **Time complexity** per command with the help of the[Big O notation](https://en.wikipedia.org/wiki/Big_O_notation) . The time complexity of the KEYS command is defined as:


> O(N) with N being the number of keys in the database…


In big databases and depending on the pattern you apply on the KEYS command this operation can lead to a long blocking Redis instance.


> **Tip** Take a dedicated look at your Redis commands and their *Time complexity* . A few commands with a high Big O estimation can slow down the performance of your Redis instance. Often there are alternative commands that serve nearly the same purpose and are a better choice (e.g. the[SCAN](http://redis.io/commands/scan) family as a replacement for[KEYS](http://redis.io/commands/keys) )


Back in 2014 there was only a small note in the[Latency generated by slow commands](http://redis.io/topics/latency#latency-generated-by-slow-commands) documentation, which said the *KEYS* command should only be used for debugging purposes. In the meantime the command reference was extended and a warning related to this was added.


Based on this new experience we had a look at our application code again. We checked all Redis commands with a special attention to the use case, used data structure and their time complexity. This was a lot of work, but it paid off. We optimized over 40% of the executed commands which led to less time spent in the communication with Redis. In the end this led to an overall faster response time of our web stack. We were fine again.


### One connection per request


We accepted the challenge with the ever growing amount of traffic in the following months and further optimized our application and stack in several ways. Some of our goals were tackling the consumption of memory per request, optimizing our database queries (slow query log), tuning our caching layers and adding more hardware (web servers) to our datacenters. Especially the last change, adding more web servers to our stack, created yet another challenge.


As mentioned earlier, we were dealing with stateless applications. Without the usage of php-fpm and persistent connections this means:


1. An HTTP request comes in
2. The application creates connections to various services
3. Operations are executed, queries are run, requests are made, etc.
4. The application closes connections created in 2.
5. The reponse is delivered to the client


This worked great so far and this is the way many applications work. But if you scale the number of servers that can accept incoming requests, your traffic grows and you don’t pay special attention to your 3rd party components this can go wrong. **Very wrong.**


Depending on the request, the application creates third party connections, executes one or two commands and disconnects again. 50% up to 75% of the commands we execute are used for connection handling. Remember Redis is single threaded. If you have a lot of clients that try to connect to your Redis instance continuously, you will keep your instance busy with connection handling instead of executing the commands you run your business logic on. This may lead to a slowdown/blocking of your Redis instance. The (simplified) image above visualizes the problem. Every arrow represents one HTTP client request.


> **Tip** Consider a proxy between your application and your 3rd party components. If you have a high connection/command ratio or Redis is used as a platform across many different teams, a proxy can be very beneficial. It can reduce the connection overhead or act as a firewall for expensive and unwanted commands.


This problem sounds like a typical proxy problem. Subsequent research showed that we were not alone and that this problem had been solved before. One solution is[twemproxy](https://github.com/twitter/twemproxy) by twitter. *twemproxy* was created specifically for this use case. You install this proxy on every webserver and twemproxy holds a persistent connection to your Redis instance(s). Your application will only connect to the local proxy which should be a lot faster, because it connects to a unix domain socket instead of an external service via network. And even better: It supports[memcached](http://memcached.org/) as well. This was good news for us, because memcached is part of our stack and might face this problem in the future.


So we introduced twemproxy into our stack. It was not a *“put it in and everything is working”* project. We had several small adjustments to make before it was a success, like


- getting[twemproxy up and running on FreeBSD](https://github.com/twitter/twemproxy/pull/293) (OS we use for web- and Redis-servers)
- adding[Redis SELECT on connect/support for multiple databases in Redis](https://github.com/twitter/twemproxy/pull/294)
- [configuring our mbuf values correctly](https://github.com/twitter/twemproxy/blob/master/notes/recommendation.md#read-writev-and-mbuf)
- check for usage of[Redis commands not supported by twemproxy](https://github.com/twitter/twemproxy/blob/master/notes/redis.md)


As described earlier the usage of multiple databases was marked as an anti pattern. At this time we were not able to move all our applications away from connecting to different databases on one Redis instance so the capability to SELECT a database other than the default one was still a requirement for us.


Another advantage of twemproxy is its ability to block expensive commands. So it can act like a circuit breaker for commands like *KEYS* and dangerous operations like[FLUSHALL](http://redis.io/commands/flushall) and[FLUSHDB](http://redis.io/commands/flushdb) .


The downside of this: Every new command of future Redis versions needs to be supported by twemproxy as well. If you upgrade your Redis installation to use new features, like[GEO commands](http://redis.io/commands#geo) , twemproxy support needs to be added and deployed as well.


The deployment of twemproxy was a great success. We eliminated all timeout and connection errors that were left (without buying new hardware).


### Bonus round: Shard your data


All known root causes for the connection and command timeouts were solved. The growth and traffic of our platform still continued and we continued to optimize our Redis usage.


Two of our Redis use cases were caching of calculated data and short term (~1 min.) storage. At some point in time one machine will not be able to handle those use cases alone anymore, because


- the data won’t fit into RAM
- the number of read/write requests is to high for one machine


As a follow up we started to shard our data over several machines using[consistent hashing](https://en.wikipedia.org/wiki/Consistent_hashing) . Luckily this way of sharding is natively supported by twemproxy. This resulted in:


- Reduction of traffic/load/requests per machine
- Improved reliability of this caching infrastructure during node failures


Both points are a big win, especially the second one. If a machine fails (e.g. hardware failure) our service is able to operate normally. Only a small percentage of compute and storage power is sacrificed. We applied this pattern to every use case where it made sense or was applicable We didn’t regret it. Most prominently, due to a node failure in this component last year this optimization had its debut.


## Conclusion


In this post, we told you our painful story of how we learned to use and benefit from *Redis* . During the time of identifying and fixing these issues, we faced multiple challenges like the constantly increasing HTTP traffic to our platform and understanding the implications of operating such a database.


Looking back this was not only a technical issue and now this seems to be obvious. The root causes of these errors were more a conceptional issue. The way we used Redis was not ideal for this kind of traffic and growth.


After understanding the issues more and more it was clear that there is no “silver bullet” to solve this problem. There were several important lessons like


- understanding how commands are executed (single threaded)
- properly configure the way how Redis persists data (BGSAVE)
- splitting data storage per service and avoiding shared Redis instances (multiple databases)
- understand time complexity of Redis commands (KEYS/O(n))
- control the amount of TCP/IP connections to your Redis instances (twemproxy)
- shard your data once it doesn’t fit onto one machine anymore and accepting machine failures (consistent hashing)


We had a hard but exciting time and all of us learned a lot. Ever since we applied the changes described here to our setup we didn’t face any bigger issue. But be aware: There are many other things you have to take care of when you run Redis in production like


- [replication and required RAM](http://redis.io/topics/replication)
- [automatic failover via sentinal](http://redis.io/topics/sentinel)
- correct use of[eviction policies](https://redis.io/topics/lru-cache#eviction-policies)
- basic understanding of[Redis security model](https://redis.io/topics/security) (also[possible attack patterns by insufficient configuration](http://antirez.com/news/96) )
- proper monitoring and alerting


Did you experience similar issues/problems? Or were you able to use any of the tips mentioned here? Let us know in the comment section.
