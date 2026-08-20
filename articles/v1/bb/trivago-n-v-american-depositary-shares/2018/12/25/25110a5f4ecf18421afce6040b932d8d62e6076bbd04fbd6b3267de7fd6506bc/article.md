---
schema_version: "1.0.0"
document_id: "25110a5f4ecf18421afce6040b932d8d62e6076bbd04fbd6b3267de7fd6506bc"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2018-12-03-teardownrebuildmovingfromhivetopyspark/"
published_at: "2018-12-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:8c9daea4771df7df999e4c25b95a51781023ee3f176230a2040e99d1f5d86514"
---

# Teardown, Rebuild: Migrating from Hive to PySpark

Machine Learning (ML) engineering and software development are both fundamentally about writing correct and robust algorithms. In ML engineering we have the extra difficulty of ensuring mathematical correctness and avoiding propagation of[round-off errors](https://en.wikipedia.org/wiki/Round-off_error) in the calculations when working with floating-point representations of a number.


As such, ML engineering and software development share many challenges… and some of the solutions to these. Concepts like unit testing and continuous integration rapidly found its way into the jargon and the toolset commonly used by data scientist and numerical scientist working on ML engineering. In 2018 it is inconceivable to trust the calculations coming from a routine without unit tests. It is even more difficult to trust a third-party numerical library that’s not tested with the same rigor as other software development projects.


At *trivago* , Data Scientists take care of developing and maintaining algorithms in production, blurring the line between a data scientist and a ML engineer. This translates into well-rounded data scientists that can equally discuss statistics and engineering concepts.


*trivago* is an auction-based marketplace. This means that advertisers place a bid to enter into an auction for the prominent position in the result page for a given hotel. *trivago’s* auction mechanism takes several factors —like price, click-through rate, and the bid itself— into account to determine the auction winner. When you visit *trivago’s* page and search for a location or keyword, a live auction mechanism runs to determine the winning advertiser, i.e. which advertiser gets to be the one on the “View Deal” button.


Bidding on *trivago* —or in any other auction-based marketplace— requires high-level expertise and a dedicated team of data scientists. *trivago’s* highest priority is to create a fair marketplace; a marketplace where both sophisticated and boutique advertisers are competitive and bring value to our users. As part of this strategic vision, *trivago* created a team specialized in bidding topics to help advertisers optimize their campaigns running in our marketplace.


I am part of the team that develops and implements bidding algorithms and strategies for *trivago’s* advertisers. These strategies aim at being efficient with respect to a business metric like return-on-advertising-spend (ROAS) or revenue-share. The first step to create an efficient bidding strategy is to estimate the true value of each click for an advertiser on *trivago’s* marketplace. We call this estimation the value-per-click (VPC).


We use Bayesian statistics to estimate the VPC for hundreds of millions of hotels and advertisers. The VPC estimation routine runs daily, incorporating evidence in the form of gigabytes of data gathered the previous day. On top of the VPC model, we developed a simple heuristic logic —called bidding layer— to calculate the final bid for the auction. Given that *trivago* is a[first-price auction](https://en.wikipedia.org/wiki/First-price_sealed-bid_auction) , an efficient bidding strategy is not only composed of the estimation of the true valuation of a click, but also requires a model of the auction environment, i.e. a model of the other participants’ valuations for the item in the auction.


## Hadoop Ecosystem and Hive


The first version of the VPC algorithm consisted of a data-pipeline that crunched data to update our VPC probability distributions by reading/writing tabular data into the Hadoop Distributed File System (HDFS) with[Apache Hive](https://en.wikipedia.org/wiki/Apache_Hive) , an SQL dialect that translates SQL into efficient[MapReduce](https://en.wikipedia.org/wiki/MapReduce) jobs.


This worked well as long as we stuck to a simple Bayesian model to calculate the VPC and simple heuristics in the bidding layer. Over time it became clear that using Hive was not a stable option to develop the bidding system we dreamed of during our sprint plannings. Working with Hive limited the team’s creativity due to the lack of expressiveness of Hive’s SQL dialect ─ other issues we found along the way changed the team’s vision of the ideal technology we wanted to develop algorithms.


## The issues


### Unit Testing


We tried different ways to write unit tests for Hive code with little success: Unit-tests on most[existing frameworks](https://cwiki.apache.org/confluence/display/Hive/Unit+Testing+Hive+SQL) must be written in Java, other frameworks were in alpha state. Instead of learning to write Java code, we decided it was way easier to replicate the production environment, copy data and configuration files to the replicated environment, and run our new algorithms there. Again, this worked fine for a while, but over time we found ourselves writing more and more temporary tables with results from intermediate calculation steps to debug the code. With this setup, the time it takes to find a bug is directly proportional to the number of intermediate tables/steps in the pipeline. And not very pleasant.


This testing practice consumed time from the team and it was not really catching important bugs. In short, our testing phase was more of a placebo than an effective technique to increase the quality of our code. Writing tests in Java was *not* an option for us.


The premise of a unit test is that it will run *fast;* in the best case, the tests will finish in a matter of a few seconds. Because we are talking about huge amounts of data, testing a single change in our algorithm meant waiting 3 to 5 hours for the pipeline to finish. Data engineers recommended sampling the data to make the testing phase run faster, but due to specific characteristics of the algorithm, sampling would result in a dramatic increase in the validation of the estimations.


### Time to production


Our issues with unit testing made the team *extremely* aware of how careful we had to be when modifying the codebase. Sometimes we had to spend days testing and validating the results of our changes —even for one-liners! This became a *major* blocker to iterate on the algorithm as fast as the team wanted.


### Expressiveness


As good and robust as Hive is for processing tabular data, our algorithm quickly evolved into calculations that demanded a programming language that allowed the team to express ideas in a simple and elegant way.


We use R for behavioral analysis, so we are accustomed to the way that[dplyr](https://dplyr.tidyverse.org/) can express tabular data manipulations in R:


```text
input_data   %>%
filter  (country  :=   'Germany'  )   %>%
mutate  (revenue  :   clicks   *   cpc)   %>%
group_by  (advertiser)   %>%
summarise  (
total_booking_amount  :   sum  (booking_amount),
total_revenue  :   sum  (revenue))   %>%
mutate  (revenue_share  :   total_revenue   /   total_booking_amount)
```


The behavioral analyses are the base for our bidding layer heuristics. Translating the results from our analyses into SQL was a frustrating process that left scars on everyone involved. For real.


Both SQL and dplyr are designed to transform tabular data, yet working with R+dplyr opens new possibilities when compared to SQL. Operations that are easy breezy on R, like manipulating dates, can become nightmare-ish and intractable on Hive/SQL.


### Speed


Well, yeah! We wanted to make the algorithm as fast as possible and Hive is infamous for being slow —there’s a price to pay for all the robustness that you get from running your pipeline in Hive, but the team was determined to push the boundaries of the status quo at *trivago* . Migrating from using MapReduce as Hive’s execution engine to[Tez](https://tez.apache.org/) already improved things but still, SQL was there!


Having a fast algorithm could open different possibilities like running the algorithm several times during the day to include recent data, or having different versions of the algorithm running in parallel for A/B testing purposes.


### Development tools


Modern programmers have IDEs at their disposal advanced that help them to catch errors before you even commit a line of code. Yet we were coding Hive scripts on a generic text editor like[sublime-text](https://www.sublimetext.com/) simply because there is no better tool to develop data pipelines with Hive. The best we could ever aspire to was to have syntax highlighting. Imagine deploying your code to the test environment just to realize, 3 hours later, that you forgot a comma on the list of output columns. Not great.


## Spark, Our Saviour


Out of the vast amount of possible technologies to write data pipelines,[Spark](https://en.wikipedia.org/wiki/Apache_Spark) emerged as one of the best options to cover the team’s needs: It has APIs to the most popular programming languages in the data science world, namely Scala, Python, R, and Java; the documentation is thorough; the community around the technology is strong; and, most importantly, it is part of the Hadoop distribution used at *trivago* , so little effort would be required to get it up and running to write the first proof of concept.


### R for Analyses, Python for Prod ®


After deciding to use Spark, and to stay clear of Scala or Java, we faced a decision that many data science teams have to make sooner or later: use R or Python? R is an amazing language for ad-hoc analyses and visualization, but it has a *big* drawback: It plays rather poorly with all the production infrastructure we have in *trivago* . In contrast, Python is a rather nice language to work with and it integrates seamlessly across different production systems. We decided for Python+Spark and never looked back.


So how well did PySpark cover the team needs?


-


**Unit testing** : As simple as using[pytest](https://www.pytest.org/) . We have two possible testing needs: functions involving data-frames, and everything else. For the first testing case, we developed an internal library that compares data-frames up to a defined numerical precision.


-


**Time to production** : Relying on the unit tests, we are now more confident in the correctness of the algorithms we write. As a consequence, a whole range of programming bugs just disappeared from our algorithms, allowing us to try new ideas much faster.


-


**Expressiveness** : Thanks to the design of[PySpark’s DataFrame API](http://spark.apache.org/docs/2.1.0/api/python/index.html) , it is straightforward to take advantage of Python’s expressiveness to manipulate data in tabular format. I personally feel relieved at having a sound library to handle dates.


-


**Development tools** : The team settled for a combination[PyCharm](https://en.wikipedia.org/wiki/PyCharm) for writing code and[Jupyter](http://jupyter.org/) to run the code interactively during the development phase.


Additionally, we decided to use type-hinting as a way to further minimize the possibility of introducing bugs or runtime errors. Using type-hints is[controversial](https://lwn.net/Articles/643269/) among seasoned Pythonistas, but we pledge allegiance to none. After unit tests, type-hints proved to be one of the best decisions we took and well worth the effort. To this day, these hints are helping us to catch that occasional misplaced argument that we would otherwise have found in later stages of development or integration testing.


### The World of UDFs


The possibility of defining bespoke functions to process tabular data, commonly known as user-defined-functions or UDFs, is what really opens new possibilities to manipulate data-frames in Spark. With a UDF it is simple to define manipulations by reasoning one row at the time instead of reasoning in terms of operations on data-frame columns.


For example, imagine you want to calculate a column to contain the conversion metric, defined as the percentage of clicks that resulted in a booking, on a table where there is a column called bookings and a column called clicks. In PySpark, it looks like this:


```text
import   pyspark.sql.functions   as   pysf


result: (
input_data
.withColumn(  'conversion'  ,
pysf.when(pysf.col(  'clicks'  )   >   0  ,
pysf.col(  'bookings'  )   /   pysf.col(  'clicks'  ))
.otherwise(pysf.lit(  None  ))
)
```


This is rather convoluted and a bit difficult to read. A programmer with no experience working with tabular data will have a hard time wrapping their head around this calculation. By using UDFs, the algorithm becomes easier to read and more robust:


```text
import   pyspark.sql.functions   as   pysf
from   pyspark.sql   import   DataFrame
from   pyspark.sql.types   import   FloatType


def   conversion_calculation  (bookings:   int  , clicks:   int  ) ->   float  :
if   clicks   is   None   or   bookings   is   None  :
raise   ValueError  (  'Faulty data'  )
if   bookings   >   clicks:
raise   ValueError  (  'Bookings greater than clicks'  )
if   clicks  :=   0  :
return   None
return   bookings   /   clicks


conversion_calculation_udf: pysf.udf(conversion_calculation, FloatType())


result: (
input_data
.withColumn(  'conversion'  ,
conversion_calculation_udf(  'bookings'  ,   'clicks'  ))
)


# The rest of your code here…
```


Besides looking better to our eyes, using UDFs makes it easier to create the unit tests for the algorithm for two reasons. First, UDFs must be[pure functions](https://en.wikipedia.org/wiki/Pure_function) which are, arguably, the easiest to test as there is no hidden state to account for. Second, mocking PySpark data-frames for unit tests is time-consuming while mocking data for a function that received primitive types is rather easy.


### The Obstacles on the Way


Spark uses lazy evaluation, which means that when a transformation is applied to a data-frame, Spark only modifies the execution “plan” and that plan is carried over only for a small set of actions, like write or count. This and the distributed nature of Spark caused us some headaches.


**UDFs Issues**


There’s a[bug](https://stackoverflow.com/questions/45095142/using-a-module-with-udf-defined-inside-freezes-pyspark-job-explanation) in the implementation of the UDF class in Spark 2.1 which will cause your algorithm to hang and freeze. There’s no error message. The job just keeps running forever. The solution is simple, i.e. delaying as much as possible the creation of the UDF. Finding the solution was not.


The tricky part of this bug is that it doesn’t manifest when running code in the spark shell. Since our development environment consisted of a Jupyter server spinning-off a Spark shell, we found this bug only when we tried to run the code on a semi-production environment.


**Joins**


The algorithm to join two data-sets in Hadoop is not trivial due to its distributed nature. The algorithm, in the worst case, transfers massive amounts of data between the cluster’s nodes \[[see details here](https://jaceklaskowski.gitbooks.io/mastering-apache-spark/spark-broadcast.html) \].


When joining a large and a small table (a few rows and columns) it is possible to broadcast the small table to all nodes to avoid the movement of the data from the large one. Spark automagically broadcasts all the tables below a predetermined size, but doesn’t always work optimally. Based on experimentation, we determined that broadcasting small tables has a *huge* impact on the overall runtime of the algorithm. For maximum control, we deactivated the automatic broadcast functionality and instead spent time identifying small-enough tables playing a role in` join` operations. We then manually forced the broadcast of such tables.


**Repartitioning Data**


More than an obstacle, this section describes an optimization technique with a big impact on the performance of the pipeline runtime.


Early on the migration phase, we found[this talk](https://www.youtube.com/watch?v=mBk4tt7AEHU) by[Kay Ousterhout](http://kayousterhout.org/) . The talk essentially challenges several common mantras deeply encroached in the Spark community. Among these mantras, the most important may be the belief that network data transfers have a significantly negative impact on the runtime of a Spark pipeline. Kay found that the impact of network data transfer is minuscule; instead, the biggest performance gains can be achieved by reducing straggler tasks, i.e. tasks that spend a significant amount of time waiting for another operation to finish.


In simplified terms, if most of the nodes in the cluster ingest roughly the same amount of information it is possible to minimize straggler tasks. Since network transfers should not be major blockers, we decided to repartition without scruple data-frames holding big amounts of data.


Repartitioning the data not only helped to minimize the straggler tasks. It also had a nice side-effect: the files written to HDFS are roughly of the same size. This, in turn, helps Hadoop to read/write these files more efficiently.


## Next Steps


Working on the transition from Hive to PySark was highly rewarding in many ways. The team’s understanding of Spark, Hive, and Hadoop increased significantly. We became better programmers and even became a better team. We managed to challenge and change some preconceptions about Spark within *trivago* . But most importantly, we ended up with a data pipeline that we love, written on a modern technology with modern programming tools. The work is not done yet: Just recently we streamlined our continuous integration pipeline to run automatically unit-tests, linting, type checking, and better automatic deployments.


For the future we are looking towards improvements in the code we wrote during the transition — I believe in constant refactoring — and to spread the knowledge across other teams within *trivago* . The door is now open to start considering algorithms that leverage Spark’s[MLLib](http://spark.apache.org/docs/2.1.0/api/python/pyspark.mllib.html) .
