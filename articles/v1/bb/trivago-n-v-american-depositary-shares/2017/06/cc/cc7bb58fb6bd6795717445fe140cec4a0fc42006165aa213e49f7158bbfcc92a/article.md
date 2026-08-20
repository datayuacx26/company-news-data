---
schema_version: "1.0.0"
document_id: "cc7bb58fb6bd6795717445fe140cec4a0fc42006165aa213e49f7158bbfcc92a"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/cucable-plugin/"
published_at: "2017-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:de9f06a3236b4aa2de0a1d9de18608b6fb5db1de19d83eaeedb80b3ba758b47d"
---

# Cucable Maven plugin for parallel execution of Cucumber scenarios

## What’s the problem?


At trivago, we are using an in-house developed[Selenium](http://www.seleniumhq.org/) framework based on[cucumber-jvm](https://cucumber.io/docs/reference/jvm#java) to run automated browser tests. As the test suite increased (the time exceeded 45 minutes for a full run), we were looking for ways to move away from sequential towards parallel execution. For Cucumber, there are actually not that many options available:


## Creating a custom runner


We tried creating a custom test runner that could parallelize scenarios. After we spent some time on research and proof of concept development we decided against it because this turned out to be really difficult as Cucumber itself does not allow much customization of its internal runner.


## Running scenarios per specific tag


Another option was giving custom tags to specific scenarios so we would be able to basically run different test suites at the same time.


```text
@suite1
Scenario: First scenario
...


@suite2
Scenario: Second scenario
...
```


That would work but is not a very flexible solution since we would be limited to the number of custom tags - three tags would mean we could only run three test threads in parallel. Also, scenario outlines with big data sets would still be considered one big scenario. Additionally, it would be hard to maintain this because with every new scenario we would have to decide to which test suite it should be added.


## Running only specific features


The Cucumber parameter *cucumber-options* allows to specify feature files or feature file locations to run. So an option was running specific features with different test runners. The major drawback of this solution is that it is hard to automate and maintain - especially in a CI pipeline. Also, a feature could potentially contain dozens of scenarios so there might not be any advantage of running it independently.


## Splitting features into smaller chunks


This sounded like a good plan. It is not super hard to do, it is much easier to automate and - depending on how small your test chunks can be - would speed up the complete runs significantly. Like any reasonable developer, we first looked at some available solutions for this like the[cucumber-jvm-parallel-plugin](https://github.com/temyers/cucumber-jvm-parallel-plugin) and the[cucumber-slices-maven-plugin](https://github.com/DisneyStudios/cucumber-slices-maven-plugin) .


Those alternatives either…


- were not flexible enough for our use case (e.g. did not consider scenario outlines with examples as separate scenarios),
- had a codebase that was too oversized and error-prone for the task (e.g. the usage of JSON marshalling and unmarshalling instead of the[Cucumber Gherkin parser](https://github.com/cucumber/cucumber/tree/master/gherkin) ).


So we took the basic idea of the existing plugins and started from scratch.


## What was the result?


We created the **Cucable** Maven plugin based on the official[Cucumber Gherkin parser](https://github.com/cucumber/cucumber/tree/master/gherkin) that does two things:


- Generate single Cucumber features from all scenarios inside of our Cucumber *.feature* files
- Generate a single Cucumber runner from a template file for every *.feature* file


## What happens after the features and runners are generated?


After the generation, we can run those files in parallel with[Maven Failsafe](http://maven.apache.org/surefire/maven-failsafe-plugin/) which lets us specify how many tests we want to run in parallel - in our case we use the` <forkCount>` option (e.g. a forkCount of 5 would mean 5 parallel runs).


More information about the various options for Failsafe parallel execution settings can be found on their[Fork Options and Parallel Test Execution](http://maven.apache.org/surefire/maven-failsafe-plugin/examples/fork-options-and-parallel-execution.html) page.


Finally, we use the[Maven Cucumber Reporting](https://mvnrepository.com/artifact/net.masterthought/cucumber-reporting) library to aggregate all generated **.json** report files into one overall test report.


## Example


Below, you can see a full example of what Cucable does.


### Source feature file


This is our source feature file. It contains a scenario and a scenario outline with two examples.


```text
Feature: This is the feature name


Scenario: First scenario
Given I am on the start page
And I click the search button
Then I see search results


Scenario Outline: Second scenario
Given I am on the start page
And I add <amount> items
And I navigate to the shopping basket
Then I see <amount> items
Examples:
| amount |
| 12     |
| 85     |
```


### Runner template file


This is the runner template file that is used to generate single scenario runners. The` \[FEATURE_FILE_NAME\]` placeholder will be automatically replaced with the name of each generated scenario.


It is possible to specify a custom runner using` @RunWith(MyCustomCucumberRunner.class)` !


By specifying *tags* , we can also split the tests even further if needed. In this case, we just ignore all tests that are annotated with` ignore` .


The *format* option tells Cucumber where to put the JSON report files for the aggregated test report.


```text
package parallel.runners;


import com.trivago.trupi.runner.MyCustomCucumberRunner;
import cucumber.api.CucumberOptions;
import org.junit.runner.RunWith;


@RunWith(MyCustomCucumberRunner.class)
@CucumberOptions(
monochrome: false,
features: {"classpath:parallel/features/<b>[FEATURE_FILE_NAME]</b>.feature"},
format: {"json:target/cucumber-report/<b>[FEATURE_FILE_NAME]</b>.json"},
strict: false,
dryRun: false,
glue: {"com.trivago.glue"},
tags: {"~@ignore"}
)
public class <b>[FEATURE_FILE_NAME]</b> {
}
```


### Generated Scenarios


For each scenario, a single feature file is created:


*Example_00001_IT.feature*


```text
Feature: This is the feature name


Scenario: First scenario
Given I am on the start page
And I click the search button
Then I see search results


# Generated by Cucable, Tue Jun 13 12:34:32 CEST 2017
```


Note that for the **scenario outlines** , each example is converted to **its own scenario and feature file** :


*Example_00002_IT.feature*


```text
Feature: This is the feature name


Scenario: Second scenario
Given I am on the start page
And I add <b>12</b> items
And I navigate to the shopping basket
Then I see <b>12</b> items


# Generated by Cucable, Tue Jun 13 12:34:32 CEST 2017
```


*Example_00003_IT.feature*


```text
Feature: This is the feature name


Scenario: Second scenario
Given I am on the start page
And I add <b>85</b> items
And I navigate to the shopping basket
Then I see <b>85</b> items


# Generated by Cucable, Tue Jun 13 12:34:32 CEST 2017
```


### Generated runners


The generated runners point to each one of the generated feature files.


This is an example for one of the generated runners - note how the placeholders are now replaced with the name of the feature to run:


*Example_00001_IT.java*


```text
package parallel.runners;


import com.trivago.trupi.runner.MyCustomCucumberRunner;
import cucumber.api.CucumberOptions;
import org.junit.runner.RunWith;


@RunWith(TrupiCucumberRunner.class)
@CucumberOptions(
monochrome: false,
features: {"classpath:parallel/features/<b>Example_00001_IT</b>.feature"},
format: {"json:target/cucumber-report/<b>Example_00001_IT</b>.json"},
strict: false,
dryRun: false,
glue: {"com.trivago.glue"},
tags: {"~@ignore"}
)
public class <b>Example_00001_IT</b> {
}


// Generated by Cucable, Tue Jun 13 12:34:32 CEST 2017
```


## Summary


Since using Cucable in our test pipeline, the test runtime dropped from 45 to about 10 minutes. This time can be decreased even more depending on the capability of the Selenium Grid and number of threads in use.


In the future, we will extend this plugin to also be able to run single features or scenarios in parallel - not only the whole set of tests. Also, we want to make it possible to run the tests multiple times in order to detect flaky tests.


If you want to check out or contribute to the Cucable project, just head over to our[Github repository](https://github.com/trivago/cucable-plugin) .
