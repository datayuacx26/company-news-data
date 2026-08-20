---
schema_version: "1.0.0"
document_id: "29e79b0a279e0b2b18c57f36728bf3fc279f55958a73efcb46182428eddce866"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/unlocking-efficient-local-development-with-dbt-2b11fb9ab40f"
published_at: "2026-02-03T09:17:43+00:00"
first_seen_at: "2026-07-20T23:17:33.321656+00:00"
fetched_at: "2026-07-28T22:22:21.045319+00:00"
content_hash: "sha256:7e5b540a572a2973d17ceba4f3b57a366810e44f3f2f84557ae5623e55ba0f38"
---

# Unlocking Efficient Local Development with dbt

# **Unlocking Efficient Local Development with dbt**


[DV Engineering](https://medium.com/@dv-engineering?source=post_page---byline--2b11fb9ab40f---------------------------------------)


10 min read


·


Feb 1, 2026


--


*Written By:*[Pinaz Shaikh](https://www.linkedin.com/in/pinaz-shaikh/)


As software professionals, we constantly strive to enhance the user experience. However, we sometimes lose sight of the fact that developers are users too and may underestimate the importance of their own experience.


According to[software.com](https://www.software.com/reports/code-time-report) , developers spend only 52 minutes daily on coding — approximately 4 hours and 21 minutes during a typical workweek. This low productivity is in large part attributed to inefficient systems, slow builds, and extensive reviews. It’s crucial that we start implementing measures to improve the development process.


My team recently integrated the Data Built Tool (dbt) into our data pipelines. One key reason for this adoption was to make the development and maintenance of our data pipelines seamless. In this article, I’ll guide you through how my team built a local dbt playground for stronger, more reliable, and confident code deployments.


## **What made us streamline local development?**


As a data engineer, my day-to-day work revolves around building and maintaining data pipelines. These pipelines are built using complex code structures with upstream and downstream task dependencies.


It is essential that we have a tool that lets us develop and test such dependencies locally. Often, a small tweak in code leads to unexpected and significant consequences that alter the software’s behavior, a phenomenon commonly referred to as “the butterfly effect.”


*“The butterfly effect rests on the notion that the world is deeply interconnected, such that one small occurrence can influence a much larger complex system.”*


This effect typically stems from poor quality in the development process. The solution lies in adapting processes and tools to mitigate such butterfly effects.


Let’s explore why we opted for the local development environment with dbt. Before integrating dbt, our typical development workflow looked like this:


Press enter or click to view image in full size


To fully test a piece of code in the staging environment, we had to go through the CI/CD process. Here are some drawbacks to this system:


1. **Inability to run end-to-end testing** : The dependencies between tasks made it difficult to understand the resulting data transformations and run end-to-end tests.
2. **Debugging was difficult** : When you have to go through CI/CD for each change, debugging anything becomes very hard.
3. **Messy merge requests** : It is imperative that we write clean and easily traceable code that keeps developers and code reviewers well-informed.
4. **Clash of coders** : Making changes to a codebase without affecting other in-flight changes may break the staging pipeline.


## Why choose dbt for local development?


With the dbt local development environment, we now don’t have to worry about all these, since we can have:


1. **Local end-to-end testing** : Each model can be built and tested locally, resulting in cleaner, more readable commits with fewer errors.
2. **Efficient debugging** : dbt provides traceable logs and builds scripts, making debugging easier. It also provides a way to debug your Jinja code by setting breakpoints.
3. **Maintaining data integrity:** dbt supports writing tests for models, which helps verify that code changes do not hamper the model’s behavior. This feature helps catch simple errors locally, rather than waiting to identify them in the CI/CD pipeline.
4. **User-specific datasets** : This allows us to test independently without breaking the staging environment for other developers.


## **Key components of the dbt local dev environment**


Several key components make the dbt local development environment more robust and reliable:


### **1. User-specific development environment**


The first component for the local development environment is to set it up as user-specific using dbt core. For this blog, I am using dbt core v1.8.2., to create different datasets that resemble the production dataset structure. Each dataset has a user_name prefix, like <user_name>_dataset_name.


### **2. Seed files**


Once you have your dataset, you need data for your models. For that you can create seed files to ingest data to the models. We aimed to create a seed file that enables the construction of test cases and a tightly bound data integrity check.


### **3. Models & macros**


With seeds ready, the next important component is to ensure that the model works well with these seed files and can be run locally. In our data pipeline, we separate the local run from the staging (STG)/production (PROD) runs using macros. Macros offer the ability to choose whether to ref() a model or seed based on the profile target.


### **4. Tests**


With everything ready, it’s essential to confirm that the models have data integrity checks in place. We can write test cases to ensure that any unnecessary code changes do not alter the model’s behavior.


By leveraging these features, dbt enhances the local development experience for data teams, promoting efficient testing and maintaining high data quality.


## **My step-by-step guide**


Now that we have a mental map of how things could work, let’s add some visuals. In this guide, we will build a data model for products and their purchase records.


The first step involves setting up a profile that creates datasets with my name as a prefix. To achieve this, we will add a user-specific profile to our dbt setup. BigQuery connection secrets are stored in a vault and accessed securely for this demo.


In the code below, I have defined a profile called *pinaz_personal* , with the target set to *personal_dev* . The output ( *personal_dev* ) defines the user-specific dataset, which has a username as a prefix for the dataset *jaffle_shop.*


dbt/profiles.yml:


```text
pinaz_personal:    target: personal_dev    outputs:      personal_dev:        dataset: pinaz_shaikh_jaffle_shop        job_execution_timeout_seconds: 300        job_retries: 1        location: US        method: oauth        priority: interactive        project: <dev-bq-project-id>        threads: 1        type: bigquery      staging:        dataset: <staging_dataset>        job_execution_timeout_seconds: 300        job_retries: 1        location: US        method: oauth        priority: interactive        project: <staging_project_id>        threads: 1        type: bigquery
```


Now you may wonder if I need to add this setup for all my datasets. The answer is “No”. Here’s why:


I want the *products* model to be in the dataset *jaffle_shop,* and the *purchase* model to be in *jaffle_shop_stg* . In the file below, you’ll see that when I define my model config, the schema that I specify here will inherit the prefix from the output dataset in profiles.yml


dbt/dbt_project.yml


In this config, we tell dbt to build all models in the *pinaz_personal/* directory as tables. These settings can be overridden in the individual model files


using the \` *{{ config(…) }}* \` macro.


```text
# Project names should contain only lowercase characters and underscores.  name: "pinaz_personal"  version: '1.0.0'  config-version: 2   # This setting configures which "profile" dbt uses for this project.  profile: "pinaz_personal"   model-paths: ["models"]  analysis-paths: ["analyses"]  test-paths: ["tests"]  seed-paths: ["seeds"]  macro-paths: ["macros", "adapters"]  snapshot-paths: ["snapshots"]   target-path: "target"  # directory which will store compiled SQL files  clean-targets:         # directories to be removed by `dbt clean`    - "target"    - "dbt_packages"    # Configuring models  # Full documentation: https://docs.getdbt.com/docs/configuring-models    models:    # Config indicated by + and applies to all files under models/pinaz_personal/purchase    pinaz_personal:      products:      purchase:        +schema: stg      purchase_summary:
```


When we run the *products* model, it creates a table inside *pinaz_shaikh_jaffle_shop,* and the *purchase* model will create a table inside *pinaz_shaikh_jaffle_shop_stg* .


```text
(venv) ➜  dbt git:(main) ✗ dbt run  21:51:07  Running with dbt=1.8.2  21:51:07  Found 3 models, 1 test, 0 snapshots, 0 analyses, 325 macros, 0 operations, 2 seed files, 0 sources, 0 exposures, 0 metrics  21:51:07    21:51:24  Concurrency: 1 threads (target='personal_dev')  21:51:24    21:51:24  1 of 3 START sql table model pinaz_shaikh_jaffle_shop.products ................. [RUN]  21:51:27  1 of 3 OK created sql table model pinaz_shaikh_jaffle_shop.products ............ [CREATE TABLE (0.0 rows, 167.0 Bytes processed) in 2.93s]  21:51:27  2 of 3 START sql table model pinaz_shaikh_jaffle_shop_stg.purchase ............. [RUN]  21:51:30  2 of 3 OK created sql table model pinaz_shaikh_jaffle_shop_stg.purchase ........ [CREATE TABLE (0.0 rows, 195.0 Bytes processed) in 2.96s]  21:51:30  3 of 3 START sql table model pinaz_shaikh_jaffle_shop.purchase_summary ......... [RUN]  21:51:33  3 of 3 OK created sql table model pinaz_shaikh_jaffle_shop.purchase_summary .... [CREATE TABLE (0.0 rows, 183.0 Bytes processed) in 2.95s]  21:51:33    21:51:33  Finished running 3 table models in 0 hours 0 minutes and 25.44 seconds (25.44s).  21:51:33    21:51:33  Completed successfully  21:51:33    21:51:33  Done. PASS=3 WARN=0 ERROR=0 SKIP=0 TOTAL=3
```


Now that we have a local environment ready with a user-specific dataset, let’s create some data for our model.


dbt/seeds/products/seed_products.csv


```text
prod_id,prod_name,stock_qty,unit_price  pd_101,prod_1,25,20  pd_102,prod_2,50,15  pd_103,prod_3,40,10  pd_104,prod_4,0,0
```


dbt/seeds/purchase/seed_purchase.csv


```text
purchase_id,prod_id,purchase_date,purchase_qty,total_price  pur_101,pd_102,2024-03-01,5,75  pur_102,pd_102,2024-03-03,2,30  pur_103,pd_103,2024-05-15,5,50  pur_104,pd_101,2024-04-14,6,120  pur_105,pd_102,2025-03-11,5,75  pur_106,pd_102,2025-03-13,2,30  pur_107,pd_103,2025-05-12,5,50  pur_108,pd_101,2025-04-15,6,120  pur_109,pd_104,2025-04-23,0,0
```


dbt/seeds/properties.yml


```text
version: 2   seeds:   - name: seed_products   - name: seed_purchase     config:       scehma: stg       column_types:         purchase_date: date
```


We will now execute the command \` *dbt seed\`*


```text
(venv) ➜  dbt git:(main) ✗ dbt seed  21:01:57  Running with dbt=1.8.2  21:01:57  Unable to do partial parsing because profile has changed  21:01:57  Found 3 models, 1 test, 0 snapshots, 0 analyses, 325 macros, 0 operations, 2 seed files, 0 sources, 0 exposures, 0 metrics  21:01:57    21:02:09  Concurrency: 1 threads (target='personal_dev')  21:02:09    21:02:09  1 of 2 START seed file pinaz_shaikh_jaffle_shop.seed_products .................. [RUN]  21:02:12  1 of 2 OK loaded seed file pinaz_shaikh_jaffle_shop.seed_products .............. [INSERT 3 in 3.55s]  21:02:12  2 of 2 START seed file pinaz_shaikh_jaffle_shop.seed_purchase .................. [RUN]  21:02:15  2 of 2 OK loaded seed file pinaz_shaikh_jaffle_shop.seed_purchase .............. [INSERT 4 in 3.23s]  21:02:15    21:02:15  Finished running 2 seeds in 0 hours 0 minutes and 18.26 seconds (18.26s).
```


We can now write models using these seed files.


In this fictional example, let’s say I want to create models that provide me with products and purchase history, specifically for purchases made in the current year.


dbt/models/products.sql


```text
{{ config(materialized='table') }}  -- products purchase in current year only  SELECT DISTINCT prod.*  FROM {{ ref('seed_products') }} AS prod  LEFT JOIN {{ ref('seed_purchase') }} AS pur USING(prod_id)  WHERE EXTRACT(YEAR FROM pur.purchase_date) = EXTRACT(YEAR FROM CURRENT_DATE())
```


dbt/models/purchase.sql


```text
{{ config(materialized='table') }}  -- current year purchase only  SELECT pur.*  FROM {{ ref('seed_purchase') }} AS pur  LEFT JOIN {{ ref('seed_products') }} AS prod USING(prod_id)  WHERE EXTRACT(YEAR FROM pur.purchase_date) = EXTRACT(YEAR FROM CURRENT_DATE())
```


Next, let’s write a new model that integrates the two tables mentioned above to calculate the total purchase quantity and price, along with the last purchase date for each product.


```text
{{ config(materialized='table') }}   SELECT  m1.prod_id,  m2.prod_name,  MAX(purchase_date) AS last_purchased_on,  SUM(purchase_qty) AS total_purchase_qty,  SUM(total_price) AS total_price  FROM ({{ generate_local_source('purchase', 'seed_purchase') }}) AS m1  LEFT JOIN ({{ generate_local_source('products', 'seed_products') }}) AS m2 USING(prod_id)  WHERE EXTRACT(YEAR FROM pur.purchase_date) = EXTRACT(YEAR FROM CURRENT_DATE())  GROUP BY ALL
```


The above code uses a macro that takes two inputs: the first is a model reference, and the second is a seed name. We filter the data source to select the target. In local development, we rely on seed data, and for staging and production, we reference the model.


dbt/macros/generate_local_source.sql


```text
(venv) ➜  dbt git:(main) ✗ dbt run   21:46:28  Running with dbt=1.8.2  21:46:28  Found 3 models, 1 test, 0 snapshots, 0 analyses, 325 macros, 0 operations, 2 seed files, 0 sources, 0 exposures, 0 metrics  21:46:28    21:46:43  Concurrency: 1 threads (target='personal_dev')  21:46:43    21:46:43  1 of 3 START sql table model pinaz_shaikh_jaffle_shop.products ................. [RUN]  21:46:46  1 of 3 OK created sql table model pinaz_shaikh_jaffle_shop.products ............ [CREATE TABLE (0.0 rows, 167.0 Bytes processed) in 2.90s]  21:46:46  2 of 3 START sql table model pinaz_shaikh_jaffle_shop_stg.purchase ............. [RUN]  21:46:48  2 of 3 OK created sql table model pinaz_shaikh_jaffle_shop_stg.purchase ........ [CREATE TABLE (0.0 rows, 195.0 Bytes processed) in 2.47s]  21:46:48  3 of 3 START sql table model pinaz_shaikh_jaffle_shop.purchase_summary ......... [RUN]  21:46:52  3 of 3 OK created sql table model pinaz_shaikh_jaffle_shop.purchase_summary .... [CREATE TABLE (0.0 rows, 183.0 Bytes processed) in 3.56s]  21:46:52    21:46:52  Finished running 3 table models in 0 hours 0 minutes and 24.04 seconds (24.04s).  21:46:52    21:46:52  Completed successfully  21:46:52    21:46:52  Done. PASS=3 WARN=0 ERROR=0 SKIP=0 TOTAL=3
```


Once we build our model, we can check BigQuery to verify our data:


products


Press enter or click to view image in full size


purchase


Press enter or click to view image in full size


purchase_summary


Press enter or click to view image in full size


Let’s write a test case to verify that the *purchase_summary* table has a product with a zero total price.


dbt/tests/purchase_summary/test_ignore_zero_price.sql


```text
select prod_id  from {{ ref('purchase_summary') }}  where prod_id = 'pd_104'  AND total_price = 0
```


```text
(venv) ➜  dbt git:(main) ✗ dbt test                                            22:26:46  Running with dbt=1.8.2  22:26:46  Found 3 models, 1 test, 0 snapshots, 0 analyses, 325 macros, 0 operations, 2 seed files, 0 sources, 0 exposures, 0 metrics  22:26:46    22:26:48  Concurrency: 1 threads (target='personal_dev')  22:26:48    22:26:48  1 of 1 START test test_ignore_zero_price ....................................... [RUN]  22:26:49  1 of 1 FAIL 1 test_ignore_zero_price ........................................... [FAIL 1 in 1.47s]  22:26:49    22:26:49  Finished running 1 test in 0 hours 0 minutes and 3.13 seconds (3.13s).  22:26:49    22:26:49  Completed with 1 error and 0 warnings:  22:26:49    22:26:49  Failure in test test_ignore_zero_price (tests/purchase_summary/test_ignore_zero_price.sql)  22:26:49    Got 1 result, configured to fail if != 0  22:26:49    22:26:49    compiled Code at target/compiled/pinaz_personal/tests/purchase_summary/test_ignore_zero_price.sql  22:26:49    22:26:49  Done. PASS=0 WARN=0 ERROR=1 SKIP=0 TOTAL=1
```


Because we have not excluded products with zero total price, the test has failed correctly. Let’s now modify the model to exclude such products.


dbt/models/purchase_summary.sql


```text
{{ config(materialized='table') }}   SELECT  prod.prod_id,  prod.prod_name,  MAX(purchase_date) AS last_purchased_on,  SUM(purchase_qty) AS total_purchase_qty,  SUM(total_price) AS total_price  FROM ({{ generate_local_source('purchase', 'seed_purchase') }}) AS pur  LEFT JOIN ({{ generate_local_source('products', 'seed_products') }}) AS prod USING(prod_id)  WHERE EXTRACT(YEAR FROM pur.purchase_date) = EXTRACT(YEAR FROM CURRENT_DATE())  GROUP BY ALL  HAVING total_price > 0
```


```text
dbt run -m purchase_summary  dbt test -m test_ignore_zero_price
```


```text
(venv) ➜  dbt git:(main) ✗ dbt test -m test_ignore_zero_price  22:30:35  Running with dbt=1.8.2  22:30:35  Found 3 models, 1 test, 0 snapshots, 0 analyses, 325 macros, 0 operations, 2 seed files, 0 sources, 0 exposures, 0 metrics  22:30:35    22:30:36  Concurrency: 1 threads (target='personal_dev')  22:30:36    22:30:36  1 of 1 START test test_ignore_zero_price ....................................... [RUN]  22:30:37  1 of 1 PASS test_ignore_zero_price ............................................. [PASS in 1.20s]  22:30:37    22:30:37  Finished running 1 test in 0 hours 0 minutes and 1.76 seconds (1.76s).  22:30:37    22:30:37  Completed successfully  22:30:37    22:30:37  Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1
```


As we can see, our test case works perfectly to filter products with a zero total price. Your dbt local development playground is now ready.


In the diagram below, you can see the development workflow with dbt.


Now that we can build all my models locally, we can test the dependencies without relying on the CI/CD to deploy changes to the Airflow DAG.


Press enter or click to view image in full size


## **Parting thoughts**


In this article, we explored the importance of streamlining local development for reliable, confident and robust code deployments. The main focus here was to demonstrate how we have leveraged dbt (Data Build Tool) to fulfill our local development needs.
I hope you can use this information as a reference point. Let me know how it works for you!
