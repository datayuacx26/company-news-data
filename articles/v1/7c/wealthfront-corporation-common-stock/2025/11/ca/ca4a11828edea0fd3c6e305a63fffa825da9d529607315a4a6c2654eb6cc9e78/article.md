---
schema_version: "1.0.0"
document_id: "ca4a11828edea0fd3c6e305a63fffa825da9d529607315a4a6c2654eb6cc9e78"
company_key: "wealthfront-corporation-common-stock"
company: "Wealthfront Corporation"
source_id: "wealthfront-corporation-common-stock-rss-9fbe6ef385e3"
canonical_url: "https://eng.wealthfront.com/2025/11/03/validuntil-ensuring-compile-time-data-integrity-in-our-investing-system/"
published_at: "2025-11-03T22:02:29+00:00"
first_seen_at: "2026-07-20T23:18:22.615485+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:e88092803d5caa8d0864239d4d38d6a2e361be0bf5197c8092c6580e6dd046ae"
---

# ValidUntil: Ensuring compile-time data integrity in our investing system

As a financial services company, it is paramount that our investing system makes decisions based on accurate, up-to-date, and valid data. A single bad input can lead to disastrous outcomes, including costly trading mistakes and a loss of client trust. At Wealthfront, we built a system called ValidUntil that allows engineers to have confidence that the compile-time data utilized by our investing system is up-to-date and appropriate for use in production.


## What is ValidUntil?


**ValidUntil** is a Java annotations-based framework that monitors and alerts on the lifecycle of compile-time datasets. In this context, compile-time datasets refer to any form of data that is checked into source code, which could include JSON files, CSV files, or member variables.


To invoke the **ValidUntil** framework, an engineer just has to create a new instance of the **@ValidUntil** annotation. The annotation itself contains metadata of the dataset it annotates. Its four fields are a **staleAt** field, an **expiredAt** field, an **author** field, and an optional **message** field for any additional information. When a dataset is stale, the system will start sending a non-urgent report to the author, and when it expires, it will escalate the alerting to an urgent page.


```text
@Target  ({ElementType.FIELD, ElementType.METHOD})
@Retention  (RetentionPolicy.RUNTIME)
public    @interface   ValidUntil {


String  staleAt  ()    default   ""  ;


String  expiredAt  ()    default   ""  ;


String  author  ()   ;


String  message  ()    default   "None"  ;


}
Code language:    Java    (  java  )
```


While designing this framework, we aimed to make it frictionless to use. Ideally, engineers would not need to register datasets or invoke any code directly. This annotations-based approach allows for seamless dataset annotation within the code itself. A significant benefit for usability is the co-location of metadata with the dataset it describes.


Here is what using **ValidUntil** might look like in practice. Consider a **RangeMap** that maps years to IRA contribution limits. Since future IRA contribution limits are not yet available, this map cannot support any keys after 2025. Consequently, any lookups for the current IRA contribution limit from 2026 onwards would yield **null** values, which would have significant downstream impacts on a client’s user experience. By annotating the dataset with **@ValidUntil** , the system will surface this to an on-call rotation before the 2026 calendar year begins.


```text
public    class    IraContributionLimitCalculator     {


@ValidUntil  (
author =  "TRADING_PRODUCTS_TEAM"  ,
staleAt =  "2025-12-01"  ,
expiredAt =  "2026-01-01"  ,
message =  "Add IRA contribution limit for 2026"
)
private    static    final   RangeMap<Year, Money> yearToIraContributionLimit = ImmutableRangeMap.<Year, Money>builder()
.put(atMost(year( 2021  )), money( 6000  ))
.put(range(year( 2022  ), BoundType.CLOSED, year( 2025  ), BoundType.CLOSED), money( 7000  ))
.build();


public    static   RangeMap<Year, Money>  getYearToIraContributionLimit  ()     {
return   yearToIraContributionLimit;
}


}
Code language:    Java    (  java  )
```


Now, starting on December 1, 2025, the Trading Products on-call rotation will start receiving a daily, non-urgent report indicating that the dataset has become stale and requires review. This report would look something like this, containing the location of the dataset, its **staleAt** date, the number of days it has been stale, as well as the optional **message** field:


If this dataset is not reviewed and updated before the **expiredAt** date, the on-call rotation would be paged to remediate this vulnerability – this is a critical state that requires immediate attention.


## Wealthfront’s Component-based Microservices Architecture


Now that we’ve seen what **ValidUntil** is, let’s take a look at how it was built. Its implementation is a great illustration of what it is like to build on top of Wealthfront’s component-based microservices architecture, which allows our engineers to package and distribute powerful functionality like this with ease.


At Wealthfront, we use an abstraction layer called “Components” to configure our microservices. These Components serve as units of configuration that define a service’s capabilities. RPC methods, referred to internally as “queries”, are bound to Components to expose endpoints that can be invoked by on-call engineers, other services, or end-users via API calls. Components are also used for configuring runtime dependency injection (via the Guice framework) and setting up event buses, queues, and cron jobs. They offer an effective way to encapsulate a system in a modular fashion, as Components frequently depend on one another. Ultimately, a service is defined within a service-level Component, which can in turn depend on other Components. The Component framework simplifies the packaging and distribution of new systems at Wealthfront, allowing us to build, iterate, and ship new systems quickly.


## The Implementation of ValidUntil


The **ValidUntil** framework is a great example of how a system can be packaged up and distributed as a Component. This is the code that defines the **ValidUntilComponent** :


```text
@Component  (
crons = {
@Cronjob  (
query = AlertOnStaleOrExpiredDatasets . class  ,
conditions     = IsLeaderQueryPredicate . class  ,
triggers     =  "0 0 9 * * ?"  )
},
modules = WReflectionsModule . class  ,
queries     = AlertOnStaleOrExpiredDatasets . class
)
public    class    ValidUntilComponent     {}
Code language:    Java    (  java  )
```


This Component defines a cron job to detect and alert on invalid datasets, a Guice module that wires up the dependencies for that job, and finally, a query that lets engineers invoke the job manually. In a more complex system, you could imagine many cron jobs, Guice modules, queries, queues, and more!


At its core, the **ValidUntil** framework utilizes runtime classpath scanning via the **org.reflections:reflections** library to discover and parse instances of the **@ValidUntil** annotation. So, the first step of the project was to create a new Guice module that would allow the daily cron job to inject its dependency on **org.reflections.Reflections** (shortened to “ **Reflections** ” for brevity). Furthermore, because runtime classpath scanning can be an expensive process, we wanted to ensure that the scanning result was cached for future instances of the cron job. We accomplished this by storing the **Reflections** object as a Guice singleton, ensuring that only one instance of the object is created on the JVM and reused for all injection requests:


```text
public    class    WReflectionsModule    extends    AbstractModule     {


@Override
public    void    configure  ()     {
bind(Reflections . class  ). toProvider  ( WReflectionsProvider  . class  ). in  ( ImmutableSingleton  . class  )  ;
}


public    static    class    WReflectionsProvider    implements    Provider  < Reflections  >   {


private    static    final   Log LOG = getLog(WReflectionsProvider . class  )  ;
private    static    final   List<String> PACKAGES_TO_SCAN = List.of( "com.kaching"  ,  "com.wealthfront"  );


@VisibleForTesting
public    static    final   Thunk<Reflections> REFLECTIONS_THUNK = thunk(() -> {
StopWatch stopWatch = LOG.getStopWatch( "WReflections construction"  , Log.Level.INFO);
LOG.info( "Pre-instantiation: %s"  , stopWatch.getElapsedTime());
FilterBuilder filterBuilder =  new   FilterBuilder();
for   (String packageToScan : PACKAGES_TO_SCAN) {
filterBuilder.includePackage(packageToScan);
}
Reflections reflections =  new   Reflections( new   ConfigurationBuilder()
.forPackages(PACKAGES_TO_SCAN.toArray( new   String[ 0  ]))
.setScanners( new   MethodAnnotationsScanner(),  new   FieldAnnotationsScanner(),  new   SubTypesScanner())
.filterInputsBy(filterBuilder));
LOG.info( "Post-instantiation: %s"  , stopWatch.getElapsedTime());
return   reflections;
});


@Override
public   Reflections  get  ()     {
return   REFLECTIONS_THUNK.get();
}


}


}
Code language:    Java    (  java  )
```


Note that we statically exposed this thunk so that our test code can access the **Reflections** object without having to construct a Guice Injector. The **@VisibleForTesting** annotation ensures that this variable is only accessed within test code.


The alerting logic for the framework is written in the **AlertOnStaleOrExpiredDatasets** class, which defines a job scheduled to run daily at 9 AM ET. To prevent duplicate executions, the job is configured with the **IsLeaderQueryPredicate** , which ensures the job is only run on one node in the cluster. This job injects the dependency on the **Reflections** object, uses it to obtain instances of the annotation, and then alerts based on the arguments of the annotation. Here is the **process** method, which contains the high-level logic:


```text
@Override
public   Unit  process  ()     {
Set<Method> annotatedMethods = reflections.getMethodsAnnotatedWith(annotationClass);
Set<Field> annotatedFields = reflections.getFieldsAnnotatedWith(annotationClass);
List<DataSet> dataSets = Sets.union(annotatedFields, annotatedMethods).stream()
.map( this  ::buildDataSet)
.sorted(Comparator.comparing(DataSet::getStaleAt))
.toList();


Multimap<Author, StaleDatasetRow> authorToStaleDatasetRows = ArrayListMultimap.create();


for   (DataSet dataSet : dataSets) {
for   (StaleDatasetRow staleDatasetRow : dataSet.toReportRowIfStale()) {
authorToStaleDatasetRows.put(dataSet.getAuthor(), staleDatasetRow);
}
dataSet.pageIfExpired();
}


maybeSendStalenessReport(authorToStaleDatasetRows);
return   Unit.unit;
}
Code language:    Java    (  java  )
```


Once the **ValidUntilComponent** was written, distributing its functionality was easy thanks to Wealthfront’s Component framework. Much of our core investing logic lives inside of a JAR called **wealthfront-investing** . **ValidUntil** was designed with the investing algorithm in mind, so it was a requirement that any instances of the **@ValidUntil** annotation inside of this JAR are picked up by the framework. So, a natural fit for this system was our read-only monitoring service, as it already has a dependency on the **wealthfront-investing** artifact and therefore has it on its runtime classpath. All we had to do to distribute this framework was add the **ValidUntilComponent** to that services’ Component dependencies section!


## Final Thoughts


In the investing world, even minor data discrepancies can have significant consequences, leading to erroneous calculations, incorrect transactions, and ultimately, a loss of client trust. **ValidUntil** tracks and alerts on the validity of all critical compile-time datasets, providing a clear and auditable trail of a dataset’s validity lifecycle. It is also a great example of how Wealthfront engineers leverage the Component framework to create systems that help us manage our clients’ money with confidence and precision.


---


Disclosures:


The information contained in this communication is provided for general informational purposes only, and should not be construed as investment or tax advice. Nothing in this communication should be construed as a solicitation or offer, or recommendation, to buy or sell any security.


All investing involves risk, including the possible loss of money you invest, and past performance does not guarantee future performance. Please see our[Full Disclosure](https://www.wealthfront.com/legal/disclosure) for important details.


Investment management and advisory services are provided by Wealthfront Advisers LLC (“Wealthfront Advisers”), an SEC-registered investment adviser, and brokerage related products, including the Cash Account, are provided by Wealthfront Brokerage LLC (“Wealthfront Brokerage”), a Member of[FINRA](http://finra.org/) /[SIPC](http://sipc.org/) . Financial planning tools are provided by Wealthfront Software LLC (“Wealthfront Software”).


Wealthfront Advisers, Wealthfront Brokerage, and Wealthfront Software are wholly-owned subsidiaries of Wealthfront Corporation.


Copyright 2025 Wealthfront Corporation. All rights reserved.
