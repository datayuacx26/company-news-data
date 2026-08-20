---
schema_version: "1.0.0"
document_id: "7eb838a6f9ca9232e1d8f4150737567274960b154256f08782d4eb66a0e80fbc"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/pipeline-quality-checks-circuit-breakers-and-other-validation-mechanisms-761fc5b1ebe4"
published_at: "2023-01-09T19:34:56+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:4f780436414ae97a74626aad400ba24f103ad32a75106b27530b4dae1d97bb44"
---

# Data Pipeline Quality Checks

Data Quality


Deequ


Pipeline


Validation


Data Validation


# Data Pipeline Quality Checks


[Elise Casey](https://medium.com/@elise_casey?source=post_page---byline--761fc5b1ebe4---------------------------------------)


10 min read


·


Jan 9, 2023


--


Press enter or click to view image in full size


Photo by[Quinten de Graaf](https://unsplash.com/@quinten149?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/photos/L4gN0aeaPY4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


This is a description of a strategy to implement data quality checks throughout a pipeline. These checks can halt the pipeline upon data quality test failures to prevent bad data from propagating to downstream tables and consumer data pipelines. While this is not the only method, it is one that has proved to be effective in improving data quality for the Analytics Platform Engineering (APE) team at[Ibotta](https://home.ibotta.com/) .


## The Problem


[Ibotta](https://home.ibotta.com/) is a data-driven company, where we treat data as a product. As with any product that a company produces, data should undergo thorough testing to ensure that it meets quality standards. These tests should help data consumers trust that the data is consistently reliable, accurate, and complete. Data is integral to making business decisions, so bad data can lead to incorrect conclusions, which could be detrimental to the company.


Before the APE team began Project Cerberus (goals: to decouple, optimize, and validate pipelines), there were only a few data validation tests in place for key tables. They were not comprehensive enough, and they did not provide insight into exactly where data bugs might have been introduced. Sometimes source tables would have bad data, which would propagate to downstream tables. Other times, a new logic change could have an unforeseen impact on downstream tables. It tends to be much easier to prevent data issues from entering tables than it is to correct those issues after the fact, so APE implemented a method to validate source tables and halt pipelines upon source data validation failures.


## The Solution


Below are the steps that APE’s[Airflow DAG](https://airflow.apache.org/docs/apache-airflow/stable/concepts/dags.html) s take to build a key rollup/fact/aggregate table:


1. Use sensors to check that source tables are up-to-date with the latest data
2. Validate source table data
— Conduct pipeline blocking checks on source data for critical quality issues
— Emit non-blocking warnings for non-critical quality issues which can be investigated during business hours
3. Build table
4. Validate final table data for known potential quality or integrity issues


Press enter or click to view image in full size


Example DAG with data quality checks


After determining that all upstream tables are up-to-date, we conduct data quality checks on those tables. These check for quality issues that might impact the integrity of the final table, so we only test the columns in the source tables that are actually used to build the final table. “Circuit-breaker” validations are upstream of the final table build task, so they halt the process that builds the table if the source table does not pass the checks. These tests are the most important ones because they impact the overall correctness of the build, such as row counts, join keys, and finance-related values. “Warning” validations are not upstream of the final table build task, so they simply emit a warning. These tests are not quite as integral to the final table’s integrity, but it is still important to be warned when columns from the source table differ from expectations.


After the circuit-breaker validations succeed, the final table builds, and then we perform some validation checks on that table. When a validation test on the final output of our build fails, warnings are emitted, but the pipeline does not halt. These checks generally test every column in the table and provide insight into whether any of the most recent data deviates from expected values. These essentially check that the business logic used to build the table is sound.


The subsequent steps of the pipeline follow a similar pattern. The screenshot below shows an example where there are multiple source tables:


Press enter or click to view image in full size


Example DAG with data quality checks on multiple source tables


This DAG


1. Ensures that all source rollup tables are up-to-date
2. Validates all relevant data from source tables (both circuit-breaker and warning validations)
3. Updates the fact table
4. Validates the fact table


This pattern continues for almost all downstream tables in the pipeline. For tables that build in multiple stages, we validate each intermediate table independently. This means that the tables stay decoupled, so they can be safely modified independently. It also makes it much easier to track down precisely where quality issues are coming from in an otherwise complex process.


## PyDeequ: the testing method


To perform these data quality validation tests, we use[PyDeequ](https://aws.amazon.com/blogs/big-data/testing-data-quality-at-scale-with-pydeequ/) , which is a Python API for Deequ. Deequ measures data quality metrics using data quality constraints in a user-defined testing suite. It then generates reports, stating whether a specific check on a column or set of columns passed or failed. Since Deequ is implemented on top of Apache Spark, it is scalable and has the ability to work with large datasets.


We wrote helper functions that accept YAML configuration files that define data quality constraints for each table column. Using those data quality constraints, the functions can run Deequ testing suites and generate reports, determining whether tests passed or failed.


## Demo: configuring DAGs with data quality tests


The APE team uses a custom-built Dag Factory library, which dynamically generates Airflow DAGs from YAML configuration files. Let’s take a look at example configuration files that generate these DAGs and data quality tests. Below is a YAML configuration file that generates a DAG using the Dag Factory.


**config.yaml**


```text
tasks:  - lvl : 1    task:    - name: "update_fact_table"      execute_via: "notebook"      new_cluster_config:        is_variable: False        cluster_config: 'dags/fact_table/cluster_config.json'      notebook_path: '/Repos/repo_name/git_repo_name/dags/fact_table/fact_table_DML'      parameters:        variables:          - "ENV"        partition: "{{ds}}"      validate_task:         - validate_fact_table        - validate_fact_table_parents      source_data:        - table: "schema.rollup_table_1"          partition_type: "none"          delta_table: True          circuit_breaker_validate_task: circuit_breaker_validate_rollup_table_1          warning_validate_task: warning_validate_rollup_table_1        - table: "schema.rollup_table_2"          partition_type: "none"          delta_table: True          circuit_breaker_validate_task: circuit_breaker_validate_rollup_table_2          warning_validate_task: warning_validate_rollup_table_2
```


We can just focus on the lines after` validate_task` , as those are the most relevant to the data quality tests in the DAG. On the lines that follow` source_data` , we define which tables are upstream of the table that we are building. In this example, those tables are` schema.rollup_table_1` and` schema.rollup_table_2` . For each of these tables, we define a` circuit_breaker_validate_task` and a` warning_validate_task` . Those values map to configurations in a circuit-breaker and warning YAML file, respectively. This is where we define what tests we want to perform on each table. Below are example configurations of each.


**circuit_breaker.yaml**


```text
circuit_breaker_validate_rollup_table_1:    email_on_failure: True    email: FAILURE_EMAIL    schema: schema    table: rollup_table_1    tests:      - type: size        threshold: 15000      # This is join key. Should be 100%      - type: complete        columns: [join_key]        threshold: 1.0   circuit_breaker_validate_rollup_table_2:    email_on_failure: True    email: FAILURE_EMAIL    schema: schema    table: rollup_table_2    tests:      - type: size        threshold: 50000      # Missing this value would impact financial reporting.      - type: complete        columns: [finance_col]        threshold: 1.0
```


**warning.yaml**


```text
warning_validate_rollup_table_1:    email_on_failure: True    email: LOW_URGENCY_EMAIL    schema: schema    table: rollup_table_1    tests:      - type: min        column: col_1        min: -50.00      - type: max        column: col_1        max: 20000.00      - type: complete        columns: [ col_2 ]        threshold: 0.99      - type: containment        column: col_3        values: [VAL_1, VAL_2, VAL_3]   warning_validate_rollup_table_2:    email_on_failure: True    email: LOW_URGENCY_EMAIL    schema: schema    table: rollup_table_2    tests:      - type: non_negative        column: col_1      - type: unique        columns: [ col_2 ]        threshold: 1.0      - type: complete        columns: [ col_2 ]        threshold: 0.99      - type: containment        column: col_3        values: [VAL_1, VAL_2, VAL_3]
```


There are fewer tests in the circuit-breaker config than the warning config. This is by design. We want to limit the circuit-breaker tests to what is absolutely necessary. This reduces noisy high-urgency alerts and unnecessary halting of the pipeline. In these examples, we only check that the source table’s row count meets baseline expectations and that join keys and finance-impacting values are completely populated.


The warning config is a bit more extensive. This is where we check all columns from the source table that are used to build the final table. In the first example (` warning_validate_rollup_table_1` ), we are checking that` col_1` fits the minimum and maximum value constraints that we defined in the config. We are also checking that` col_2` is 99% non-null and that` col_3` only contains values` VAL_1` ,` VAL_2` , or` VAL_3` . These are set as warnings because perhaps` col_1` can expand beyond the set min and max values over time, or` col_3` could begin to contain another value` VAL_4` and still be valid. As for` col_2` , there could already be some nulls in that column that are still valid data, and we just want to check that the null rate of that column does not increase abruptly.


In the second example (` warning_validate_rollup_table_2` ), we are checking that` col_1` only contains values greater than or equal to 0, that` col_2` is 100% unique and 99% non-null, and that` col_3` only contains values` VAL_1` ,` VAL_2` , or` VAL_3` . By sending low-urgency warnings, APE team members are alerted to determine if the data that failed test is actually bad data, or if it is something new and expected in the source data, so the tests should be modified.


**validate.yaml**


```text
validate_fact_table:    email_on_failure: True    email: LOW_URGENCY_EMAIL    schema: schema    table: fact_table    tests:      - type: unique        threshold: 1.0        columns: [col_1]      - type: complete        threshold: 1.0        columns: [col_1]      - type: complete        threshold: 0.9999        columns: [col_2]      - type: complete        threshold: 0.9999        columns: [col_3]      - type: containment        column: col_3        values: [VAL_1, VAL_2]  validate_fact_table_parents:    email_on_failure: True    email: LOW_URGENCY_EMAIL    schema: schema    table:      SELECT COUNT(*) AS no_parent_count      FROM schema.fact_table f      WHERE f.id NOT IN (SELECT id FROM schema.monolith_table);    tests:      - type: max        column: no_parent_count        max: 0
```


One line of the config.yaml file defines a` validate_task` . This is the task that will validate the final table’s data. The values assigned to` validate_task` (` validate_fact_table` and` validate_fact_table_parents` ) map to configurations in a final YAML file called validate.yaml (example shown above). The first config (` validate_fact_table` ) has general tests that check for unique and complete columns that only contain a specific set of values. The second config (` validate_fact_table_parents` ), is a bit more specific. This one queries from the fact table that was just built and checks if each ID maps to an ID in` schema.monolith_table` . These tests are easily customizable, so we can test just about anything.


### What do we do if circuit-breaker tests fail on source data?


Press enter or click to view image in full size


Real example of a DAG with a failed circuit-breaker validation task Press enter or click to view image in full size


Output from failed circuit-breaker validation tests


The screenshots above are from a real example of an APE DAG that had a failed circuit-breaker data validation task on 2022–12–25. This validation task failed on a` size` check, stating that “Value: 1312 does not meet the constraint requirement.” We set this test to fail if the daily number of source table rows was ever fewer than` 24000` . In this case,` 1312` is much lower. So what happened? When the pipeline halted with this test failure, it alerted the person that was on-call. They looked at the process that builds the source table, and confirmed that everything had executed successfully. This low volume of records was actually valid. This failure happened on Christmas day, so there were simply fewer consumer purchases due to store closures for the holiday. After confirming that this was the case, the on-call person marked the task as success to allow the pipeline to continue.


Prior to implementing these circuit-breaker validation tests, there was an incident where a process was running behind that builds the source tables that APE’s pipeline depends on. The upstream process had partially completed, so to APE’s table sensors, it seemed like it was safe to begin the pipeline. Unfortunately, this data was incomplete. If we had had our circuit-breaker validation tests implemented at this time, the pipeline would have halted with a failed` size` (row count) test. Instead, we had to rerun our pipeline when we discovered what had happened, which resulted in an increase in cost and delayed availability of data that day. Circuit-breaker validation tests would have prevented this! (Side note: We have put additional measures in place since then to prevent this from happening again).


### What do we do if warning tests fail on source data?


When warning validation tests fail, we investigate the failed columns, and determine if the data seems flawed. If so, we let the upstream data producers know during business hours. We also notify data consumers that the table may need reprocessing pending feedback from producers. If they tell us that this is the expected behavior, we modify our tests so that they pass in the future. If not, we work with the data producers to account for the issue in the existing data and ensure that it meets quality expectations moving forward. This is very rare, as we have a process for validating event data prior to making it available for use in downstream builds. These are set as low-urgency alerts because there is very little we can do in the middle of the night when it is an issue with upstream data from another team.


### What do we do if validation tests fail on the final table?


When these tests fail, we check if there was a recent logic change in the script that builds the table. If so, we have to dig in to see how exactly that logic change is affecting any failed column tests, and if that logic is flawed. If so, we work on fixing that logic. If not, we modify the tests to reflect the data’s new behavior. Either way, it is good to be warned about the new behavior before data consumers find potential issues.


## Conclusion


It is considered best practice to incorporate data quality tests into pipelines. It is especially beneficial to have data quality tests that can halt the progress of a pipeline upon source data test failures. These tests can prevent bad data from being inserted, which minimizes time and resources spent resolving data issues. Having multiple data quality checkpoints throughout each DAG in a pipeline provides data consumers with the peace of mind that they are making reliable data-driven decisions for the company.


*Interested in working at Ibotta? Check out*[https://ibotta.com/careers](https://ibotta.com/careers)
