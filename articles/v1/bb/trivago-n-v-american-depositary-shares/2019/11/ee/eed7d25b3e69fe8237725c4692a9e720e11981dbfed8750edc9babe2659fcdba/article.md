---
schema_version: "1.0.0"
document_id: "eed7d25b3e69fe8237725c4692a9e720e11981dbfed8750edc9babe2659fcdba"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2019-11-14-apitestautomationusingkarate/"
published_at: "2019-11-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:61990be9cd664799dc2274f2bcde976ef6e08b22d8255f3c6d1b6599f398d714"
---

# Automation-First Approach Using the Karate API Testing Framework

Adopting an automation-first mindset is the first step to reduce manual and repetitive work. Thinking this way enables us to move faster, and more efficiently. It unburdens us from mundane, repetitive work, allowing us to focus on solving problems and creating value in the Software Development Life Cycle.


So the first thing is to look for a tool that helps us write automated tests faster and is easy to maintain.


Karate is an open-source API test automation tool. API tests are written using Behaviour Driven Development (BDD)[Gherkin](https://cucumber.io/docs/bdd/better-gherkin/) syntax. Unlike other BDD frameworks like[Cucumber](https://en.wikipedia.org/wiki/Cucumber_(software)&sa=D&ust=1572942289138000) ,[Specflow](https://testingbot.com/support/getting-started/specflow.html) or[JBehave](https://www.baeldung.com/jbehave-rest-testing) , Karate has all the[step definitions](https://cucumber.io/docs/cucumber/step-definitions/) written for us so we don’t have to worry about writing them. This enables even non-developers to easily write API tests for the services. Let’s check some of the main features of the framework and how it makes API test development easier.


## Setting up a Karate project


Assuming you have Java and Maven installed on your machine, setting up Karate is very easy with the Karate Maven archetype plugin.


1. Open a command-line interface (CLI)
2. Enter the following command :


```text
mvn archetype:generate
"-DarchetypeGroupId=com.intuit.karate"
"-DarchetypeArtifactId=karate-archetype"
"-DarchetypeVersion=<version>"
"-DgroupId=karatetests"
"-DartifactId=karatetests"
"-Dpackage=karatetests"
"-Dversion=1.0.0-SNAPSHOT"
"-DinteractiveMode=false"
```


1. A project folder will be generated with name` karatetests` .
2. Launch IntelliJ or Eclipse (or any other IDE)
3. Go to` File > Open > karatetests`


If you have an existing Java Maven Project, you could add the following dependencies to the project:


```text
<  dependency  >
<  groupId  >com.intuit.karate</  groupId  >
<  artifactId  >karate-apache</  artifactId  >
<  version  >${karate.version}</  version  >
<  scope  >test</  scope  >
</  dependency  >


<  dependency  >
<  groupId  >com.intuit.karate</  groupId  >
<  artifactId  >karate-junit4</  artifactId  >
<  version  >${karate.version}</  version  >
<  scope  >test</  scope  >
</  dependency  >
```


## Create Some Api Tests


1.


Under` src/test/java` , create a feature file called` test.feature`


2.


Enter the following: Feature: Demo Karate Tests Scenario: Verify that test server is up and running Given URL[http://localhost:8000/api/json?pretty=true](http://localhost:8000/api/json?pretty=true) When method get Then status 200 And match response.id:= 2


3.


Execute test


The test should be successful!


Here, as you can see, you don’t even need to write any Java code as Karate sits on top of cucumber and it inherits all the cucumber features. You can create your API tests in BDD syntax without the overhead of writing any code to implement your scenarios.


## What’s so Cool about Karate?


No compilation and programming knowledge needed!


Karate provides its own DSL (Domain Specific Language), which uses a Gherkin-like language enabling one to write tests without programming knowledge and write tests in` .feature` files. Here is slightly more advanced one:


```text
Feature  :   sample karate test script
Background  :
*   url 'https://...'
Scenario  :   get all customers and then get the first customer by id
Given path 'customers'
When method get
Then status 200
*   def first  :   response[0]
Given path 'customers', first.name
When method get
Then status 200
```


The Given/When/Then format for scenarios makes it easy to read and understand. Also, one does not need to know much about the internal Java implementation; making it easier to write tests.


## Using Custom User-defined Functions


Most of the code is written in Karate. But there are cases where you need to perform custom actions like generating test data like a random string, a date, numbers, or tokens. In these cases, Karate offers flexibility to write custom functions using Java or JavaScript and one can directly call them in the feature files.


Calling any Java code is easy. Given a custom, user-defined Java class:


```text
public   class   TestDataGenerator
{
private   static   final   String defaultLocale  :   "en-GB"  ;
public   static   long   number  ()
{
int   offset  :   (  int  ) (Math.  random  ()   *   1000  )   +   1  ;
return   System.  currentTimeMillis  ()   +   offset;
}


public   static   String   uuid  ()
{
return   UUID.  randomUUID  ().  toString  ();
}


public   static   Faker   faker  ()
{
return   TestDataGenerator.  faker  (TestDataGenerator.defaultLocale);
}


public   static   Faker   faker  (String   locale  )
{
return   new   Faker  (  new   Locale  (locale));
}


public   static   String   futureDate  (  int   days  , String   format  )
{
Date now  :   new   Date  ();
Date date  :   new   Date  (now.  getTime  ()   +   TimeUnit.DAYS.  toMillis  ((  long  ) days));
SimpleDateFormat simpleDateFormat  :   new   SimpleDateFormat  (format);
return   simpleDateFormat.  format  (date);
}
}
```


This is how it can be called from a test-script:


```text
Feature  :    Availability Request
Background  :
*   configure lowerCaseResponseHeaders  :   true
*   def testData  :   Java.type('com.trivago.eb.util.TestDataGenerator')
Scenario  :   I perform successfully an availability request to a customer endpoint
*   def startDate  :   karate.get('startDate') != undefined ? startDate   :   testData.futureDate(14, 'yyyy-MM-dd')
*   def endDate  :   karate.get('endDate') != undefined ? endDate   :   testData.futureDate(15, 'yyyy-MM-dd')
Given url endpoint
And path '/customer_availibility'
And form field start_date  :   startDate
And form field end_date  :   endDate
When method post
Then status 200
```


## Better Support for Debugging Tests


As Karate does not require an IDE to write tests, and the tests are written in` .feature` files, debugging test failures can be tricky; but recently there is support for this using the[VS Code Karate](https://marketplace.visualstudio.com/items?itemName=kirkslota.karate-runner) plugin.


## GraphQL Testing Support


The framework provides amazing[GraphQL](https://graphql.org/learn/) testing support, making it easy to perform requests and verify complex and dynamic JSON responses. Let’s see an example…


```text
Feature  :   Test GraphQL endpoint
Background  :
*   url 'https://test.server/graphql'
Scenario  :   Simple GraphQL request
Given text query  :
"""
{
customer(id: 101) {
id
name
}
}
"""
And request { query  :   '#(query)'   }
When method post
Then status 200


*   match $.data.advertiser.id:= 100
*   match $.data.advertiser.name:= 'Automator'
*   match $.data.advertiser.supplierId:= 100
```


In line 4, we are setting up the URL to our GraphQL server. Then we are calling the GraphQL query in the` Given` statement. The following line shows how to build a request with an` And` statement and finally how the JSON is validated in the` * match` statement.


Apart from these, there are many other cool features Karate provides. You can feed data from a JSON array and perform[Dynamic Data-Driven testing](https://github.com/intuit/karate#data-driven-tests) at runtime. Matching the full payload in a single line, as well as updating the JSON payload is very easy. As mentioned above, Karate is easy to set up using Maven. It’s therefore also very easy to integrate into your CI pipeline. One can run tests regularly and receive fast feedback on newly added check-ins. You may have a look at all the features over[here](https://github.com/intuit/karate) .


## What’s Next?


We have been writing automated tests using Karate for quite some time now and are happy with the results. We will focus on improving our automation coverage next and find more areas where we can reduce the effort of manual testing. Building and maintaining automated tests is a lot of work. To get a good return on investment, we will be using these measures to evaluate our testing:


- Finding real problems: Your Automated tests should find enough real problems.
- Avoiding false alarms: Many times a test will show a failure because of an intended change, and you will have to go back and modify the test.


So if your team is looking for a testing tool for your project to write API tests faster and without much programming knowledge, Karate might be worth a try.
