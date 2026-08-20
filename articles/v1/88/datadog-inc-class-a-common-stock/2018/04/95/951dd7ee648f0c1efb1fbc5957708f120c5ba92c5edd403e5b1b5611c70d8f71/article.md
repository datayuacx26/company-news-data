---
schema_version: "1.0.0"
document_id: "951dd7ee648f0c1efb1fbc5957708f120c5ba92c5edd403e5b1b5611c70d8f71"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/using-datadog-apm-to-find-bottlenecks-and-performance-benchmarking/"
published_at: "2018-04-24T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:8228540953dfd41d813317c9fff1deb0cf2a5d7520da7c25a388121e91fc4b44"
---

# Using Datadog APM to improve the performance of Homebrew

Andrew Robert McBurney


As a Software Engineering Intern on the open source team at Datadog, I contribute to various open source projects using Datadog tools to track down bugs and performance related bottlenecks.


One of the tools I use is[Application Performance Monitoring](https://www.datadoghq.com/product/apm/) (APM). APM allows you to instrument your code with a client tracing library, letting you view the breakdown of method performance across your application. It’s effective at finding performance bottlenecks, and for comparing the performance of mutually exclusive implementations when making a design decision.


## Homebrew


The first open source project I contributed to at Datadog was[Homebrew](https://www.brew.sh/) : a popular package management system for macOS. Being a macOS user for many years, I’ve used Homebrew on numerous occasions; I wanted to give back to the project which saved me countless hours of installation and configuration headaches.


Homebrew is used extensively by the engineering teams at Datadog, as it’s part of the standard developer toolkit. Because of this, improving the performance of Homebrew saves engineers time across the entire organization.


` brew linkage` is a development-specific Homebrew command used to check the library links of an installed formula (Homebrew’s term for a package).


There was an open[GitHub issue](https://github.com/Homebrew/brew/issues/3008) with a` help-wanted` label seeking to make` brew linkage` faster. A performant` brew linkage` command could be run automatically to detect when a` brew reinstall` is needed because the user is using an optional dependency.


From the[GitHub issue](https://github.com/Homebrew/brew/issues/3008) , the requirements for the solution were:


> Ideally on an SSD and *modernish* hardware it should be able to scan ~50 packages (with some large ones like` boost` ) in under 5 seconds.


## Investigation of performance bottleneck


I instrumented Homebrew’s code using Datadog’s[ddtrace gem](https://github.com/Datadog/dd-trace-rb) for Ruby.


**Gem usage**


The gem allows users to track the performance of blocks of code they trace:


```text
1   require     "ddtrace"    2
3   def     some_function    4       # The time spent to complete this 'block' is tracked by the APM    5       Datadog  .  tracer  .  trace  (  "some_function"  ,   service:     "my_app"  )   do    6         sleep     2    7         Datadog  .  tracer  .  trace  (  "some_expensive_operation"  ) {   sleep     3   }    8         sleep     1    9       end    10   end
```


From here, users may navigate to the APM user interface to view traces in a flame graph:


**Usage in Homebrew**


To pinpoint the bottleneck, I instrumented Homebrew from the ‘root’ point of execution (in` Library/Homebrew/dev-cmd/linkage.rb` ) to account for the command’s total execution time and all the subsequent methods called by the` linkage` function.


Analyzing the trace data in a flame graph, I was able to pinpoint the source of the performance issues to the` check_dylibs` function in the` LinkageChecker` class.


*Flame graph showcasing the breakdown of time spent for methods instrumented during execution.*


## Optimization solutions


I investigated a solution utilizing multi-threading to speed up the processing of the` LinkageChecker::check_dylibs` function. I used Ruby’s` Thread` primitives to see if concurrency would get the desired performance. However, the limitations of the[GIL](https://en.wikipedia.org/wiki/Global_interpreter_lock) made this solution inviable and did not meet the required performance specification.


I looked into viable alternatives to improve performance and decided to implement a caching mechanism using[SQLite3](https://www.sqlite.org/) : an on-disk light database used for persistent storage.


### Initial solution


**` SQLite3`** **caching implementation**


The most expensive operation for the` brew linkage` command was the processing required in the` LinkageChecker::check_dylibs` function. The function appends path locations to arrays corresponding to the type of library or dependency (i.e.,` system_dylibs` ,` broken_dylibs` ,` undeclared_deps` , etc.).


I modeled a single-table schema for caching relevant linkage information required by the` LinkageChecker` . The data fetched could be post-processed to meet one of the two formats expected by the printer function. The schema was defined as follows:


```text
1   CREATE     TABLE     IF     NOT     EXISTS   linkage (    2        id   INTEGER     PRIMARY KEY   AUTOINCREMENT,    3       name     TEXT     NOT NULL  ,    4       path     TEXT     NOT NULL  ,    5       type     TEXT     NOT NULL     CHECK   (  type     IN   (    6         'system_dylibs'  ,   'variable_dylibs'  ,   'broken_dylibs'  ,   'undeclared_deps'  ,    7         'unnecessary_deps'  ,   'brewed_dylibs'  ,   'reverse_links'  )),    8        label   TEXT     CHECK   (label   IS     NULL     OR   (  type     IN   (  'brewed_dylibs'  ,   'reverse_links'  ))),    9       UNIQUE  (  name  ,   path  ,   type  , label)   ON   CONFLICT   IGNORE    10   );
```


Internally, the application could insert, mutate or fetch data from the cache using a SQL-syntax:


```text
1   db   =   SQLite3::Database  .  new     "  #{  HOMEBREW_CACHE  }  /linkage.db"    2
3   # Storing path data in the cache for a particular key and linkage type    4   db.  execute  (    5       <<~SQL    6         INSERT INTO   linkage (  name  ,   path  ,   type  )    7           VALUES   (  'key'  ,   'path_name'  ,   'linkage_type'  );    8        SQL    9   )    10
11   # Fetching path data from the cache for a particular key and linkage type    12   db.  execute  (    13       <<~SQL    14         SELECT     path     FROM   linkage    15           WHERE     type   =   'linkage_type'    16           AND     name   =   'key'  ;    17        SQL    18   ).  flatten
```


#### Performance enhancements


I noticed significant performance gains with the caching in-place. Below are two flame graphs showcasing the breakdown of execution time for the command with 106 packages (including larger packages such as` boost` ). The command ran in under 5 seconds with the caching mechanism utilized, as requested in the requirements.


#### Flame graphs


**<small>Without the optimization</small>**


Without the caching, the command took 11.5 seconds to complete processing 106 packages. The library` boost` took 1.01 seconds to complete checking the dynamic libraries.


**<small>With the optimization</small>**


With the caching, the command took 182 milliseconds to complete processing 106 packages. The library` boost` took 1.38 milliseconds to complete checking the dynamic libraries.


### Second solution


**` pstore`** **caching implementation**


After the initial implementation, I[submitted a Pull Request](https://github.com/Homebrew/brew/pull/3720) to Homebrew, asking for a code review from the maintainers. It was recommended I try a different implementation using Ruby’s[pstore](https://ruby-doc.org/stdlib-1.9.2/libdoc/pstore/rdoc/PStore.html) , a file-based persistence mechanism based on Ruby’s` Hash` . The principle advantage of using` pstore` over` SQLite3` is that it doesn’t require a third-party gem dependency.


I[created a branch](https://github.com/AndrewMcBurney/brew/pull/1) off of my` SQLite3` implementation to compare the performance of using` pstore` over` SQLite3` as the caching mechanism. The abstractions from the first implementation made it easy to swap out the` SQLite3` -coupled database code for the new` pstore` implementation.


My goal was to keep the public interface of the` LinkageStore` class as similar as possible for the` pstore` implementation for comparison reasons. However, I did not need to define a schema for the cache since` pstore` stores Ruby objects to the data store file with[Marshal](http://ruby-doc.org/core-2.2.2/Marshal.html) : a library which converts Ruby objects into a byte stream.


Below is an example of fetching and mutating data for the` pstore` implementation:


```text
1   db   =   PStore  .  new     "  #{  HOMEBREW_CACHE  }  /linkage.pstore"    2
3   # Storing path data in the cache for a particular key and linkage type    4   db.  transaction     do    5        db[  "key"  ] = {   "linkage_type"   =>   "path_name"   }    6   end    7
8   # Fetching path data from the cache for a particular key and linkage type    9   db.  transaction     do    10        db[  "key"  ][  "linkage_type"  ]    11   end
```


### Comparison of SQLite3 and pstore


The` pstore` implementation has nicer syntax for storing and fetching data using Ruby, since` pstore` transactions have a similar syntax to Ruby’s native` Hash` get/set syntax, whereas the` SQLite3` implementation uses a SQL syntax.


However, the biggest downside to the` pstore` implementation is the performance for fetching and persisting data. Using` pstore` caching is slower than the original implementation without any caching. Persisting the Marshaled byte-stream data on-disk and fetching the data then loading it into a Ruby object are expensive operations.


With caching, the command took 42.2 seconds to complete *for just two packages!* One` fetch_path_values!` method call *(illustrated in the graph below)* took 3.51 seconds total. For comparison, this took more time fetching information from two packages than the` SQLite3` implementation took fetching information from 106 packages.


I recommended to the maintainers we use` SQLite3` over` pstore` for the caching mechanism, as I believed the tradeoff of installing another gem was worth the performance benefits of using` SQLite3` for caching.


### Third solution


**` DBM`** **caching implementation**


After investigating the performance differences of the` pstore` and` SQLite3` implementations, it was recommended by the maintainers to look into an implementation using a[Berkeley DB](https://web.stanford.edu/class/cs276a/projects/docs/berkeleydb/ref/intro/dbis.html) . A Berkeley DB is another on-disk persistent storage database which is commonly used for caching. Ruby provides a module for interfacing with a Berkeley DB called[DBM](https://ruby-doc.org/stdlib-1.9.3/libdoc/dbm/rdoc/DBM.html) , which like` pstore` , doesn’t require a third-party gem dependency.


Inserting and mutating data from the cache was similar to the` pstore` implementation, but with some extra parsing needed for fetching, since all Ruby hashes and arrays are persisted as strings:


```text
1   db   =   DBM  .  open  (  "  #{  HOMEBREW_CACHE  }  /  #{  name  }  "  ,   0666  ,   DBM::  WRCREAT  )    2
3   # Storing path data in the cache for a particular key and linkage type    4   db[  "key"  ] = {   "linkage_type"   =>   "path_name"   }    5
6   # Fetching path data from the cache for a particular key and linkage type    7   JSON  .  parse  (db[  "key"  ].  gsub  (  "=>"  ,   ":"  ))[  "linkage_type"  ]
```


### Comparison of SQLite3 and DBM


#### origin/master


**<small>Without the optimization</small>**


The baseline without caching *(i.e., the time it takes for* *` brew linkage`* *to complete on* *` origin/master`* *)* took 8.04 seconds to complete for 99 packages. The library` boost` took 704 milliseconds to complete checking the dynamic libraries and printing the output.


#### SQLite3


**<small>Without the optimization</small>**


Without the caching, the command took 10.3 seconds to complete for 99 packages. The library` boost` took 777 milliseconds to complete checking the dynamic libraries and printing the output.


**<small>With the optimization</small>**


With the caching, the command took 180 milliseconds to complete for 99 packages. The library` boost` took 1.77 milliseconds to complete checking the dynamic libraries and printing the output.


#### DBM


**<small>Without the optimization</small>**


Without the caching, the command took 8.14 seconds to complete for 99 packages. The library` boost` took 686 milliseconds to finish checking the dynamic libraries and printing the output.


**<small>With the optimization</small>**


With the caching, the command took 208 milliseconds to complete for 99 packages. The library` boost` took 2.15 milliseconds to complete checking the dynamic libraries and printing the output.


#### Summary


I’ve summarized the performance comparison between the` SQLite3` implementation and the` DBM` implementation below:


Implementation No Optimization Cache Optimization


` origin/master` 8.04 seconds N/A


` SQLite3` 10.3 seconds 180 milliseconds


` DBM` 8.14 seconds 208 milliseconds


#### Observations


-


It took longer to build the cache (i.e., the *No Optimization* column) for both` SQLite3` and` DBM` implementations than it currently takes on` origin/master`


-


The` DBM` implementation built the cache faster than the` SQLite3` implementation


-


It’s much faster to run the` brew linkage` command when the values are cached (i.e., the *Cache Optimization* column for` DBM` and` SQLite3` )


-


The` SQLite3` implementation is faster at accessing values from the cache than the` DBM` implementation *(but not by much)*


-


The implementation met the requirements specification, by processing > 50 packages in under 5 seconds


## Recommendations


I recommended proceeding with the` DBM` implementation due to the following:


-


The` DBM` solution doesn’t require a vendored gem like the` SQLite3` implementation


-


The` SQLite3` gem requires building native extensions from C with[rake/extensioncompiler](https://github.com/rake-compiler/rake-compiler)


-


Setting the cache is slightly faster than the` SQLite3` implementation, and while the` SQLite3` solution is faster at fetching data from the cache, it’s not faster by much


-


Cleaner to read the code and understand the cache structure/schema


-


Fetching and mutating data uses Ruby’s` Hash` get/set conventions over a SQL syntax


## Conclusion


When most people think of Datadog, they probably think of cloud infrastructure monitoring, metrics, or alerting. Application Performance Monitoring opens up a variety of different use-cases including performance benchmarking, bug squashing, and finding bottlenecks in your applications.


Making data-driven decisions is an essential part of being an engineer. The performance data collected from the APM helped to guide my engineering judgment when implementing features and making design decisions.


APM provides a means to show both technical and non-technical stakeholders how an application performs. The graphs in the user interface make it easy to see which parts of your application take the most time, allowing for an engineer to optimize and compare the performance of their changes.


---


*Update* : Andrew returned to the University of Waterloo following his internship, but continued to work on caching in Homebrew. We’re thrilled to announce that his Pull Request was merged and should be included in the upcoming release of Homebrew (1.6.5).


---


#### Datadog 💜 open source


Datadog is a proud user of and contributor to many open source projects. As part of our continued commitment to open source, we’re always looking for ways we can give back to the community.


If you have an open source project or initiative that you feel would benefit from application performance metrics discussed in this article, we’d like to help. You may[apply here](https://www.datadoghq.com/partner/open-source/) for free access to Datadog.


-
-
-
