---
schema_version: "1.0.0"
document_id: "158eee21b837ded15b8db9522c6277d283c92576550fcc5c6b58450430b5d450"
company_key: "similarweb-ltd-ordinary-shares"
company: "Similarweb Ltd."
source_id: "similarweb-ltd-ordinary-shares-rss-2bd7dd1f88a6"
canonical_url: "https://medium.com/similarweb-engineering/how-we-healed-our-airflow-env-ce58783968b0"
published_at: "2023-02-08T08:40:39+00:00"
first_seen_at: "2026-07-20T23:19:16.581763+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:ca3d8ad0d97464452461b3d4addf75238c7fbc4c9dc3b75fed3881b40c5e90b7"
---

# How we healed our AWS MWAA (aka airflow) env

# How we healed our AWS MWAA (aka airflow) env


[Lior Mor](https://medium.com/@lior.mofr?source=post_page---byline--ce58783968b0---------------------------------------)


5 min read


·


Feb 5, 2023


--


Press enter or click to view image in full size


**tl;dr** in order to save your airflow’s scheduler CPU:
1. Use imports only where you need it. Separate code to files and reduce many redundant dependencies and CPU consumption.
2. Remove network and db calls from the dag processing.
3. Use the scheduler environment variables in order to enhance the scheduler work.
4. Use .airflowignore file


[Apache Airflow](https://airflow.apache.org/) is a great product to arrange, configure and orchestrate our data pipelines. In a data company such as Similarweb, it is essential to maintain a single system that enables us to update and monitor our ETLs with, and airflow gives us such solution.
In the last months we moved from on-premise cluster to MWAA — a managed version of airflow running in AWS, which simplifies the usage by letting AWS do some stuff instead of the developers, such as monitor and report, auto scaling and other integrations.


Press enter or click to view image in full size


key part in Airflow architecture is the scheduler, a micro service that handles dags (dag — **d** irected **a** cyclic **g** raph, that represents execution of tasks and their dependency on each other) and tasks execution, with respect to its dependencies, i.e. time schedule and other tasks execution.


Through the time, when adding more and more dags and expanding the infrastructure for dag processing, we found out that the CPU consumption became higher and higher.
The problem started on the days we used the on-prem environment. When moving to MWAA, we hoped that the managed env will solve it by its auto-optimization. however when we started migrating and adding the dags to MWAA we found out that the problem is still here and the scheduler’s CPU is always on 100%. As long as most of the dags not running we barely felt it however when lots of dags were triggered the scheduler just couldn’t trigger fast enough and many tasks failed to start, which caused very high latency and even throttled actions. dags could not render.


First thing we made is, along with the MWAA support, understand better the accessible environment variables that control the scheduler’s actions and load. controlling those variables in MWAA is just simple and can be done through the page of your MWAA environment in AWS console. for those who use airflow on-premise it can be done by the airflow.cfg file. There are the main values we change:
1.` scheduler.min_file_process_interval` — this value reflects the maximum time in seconds that the scheduler will process each dag file. As you can guess, lower number means higher cpu. we increased it from 30 seconds to 300.
2.` core.min_serialized_dag_update_interval` — the minimum interval in seconds which a dag state will be updated in the airflow database, here we also increased from 30 to 300 seconds.
3.` core.sql_alchemy_pool_size` — number of max connections in a database pool. we raised it from 5 to 25 in order to make up for the scheduler’s interval, and put more load on the network than on the CPU.
4.` scheduler.scheduler_idle_sleep_time` —since the default for the scheduler to sleep within loops is only 1 second, we raised it and set it to 5.
for more info on how to control airflow core services go[here](https://airflow.apache.org/docs/apache-airflow/2.2.2/configurations-ref.html)


The second thing we done was to add` .airflowignore` file in the dags s3 bucket.
with MWAA you define the s3 bucket where your dags are, which promises by MWAA to update the environment with any change without the need to close and deploy the env over and over. The scheduler constantly scans this bucket to process dags and/or update them. Since we hold other files, that are not dag files in this bucket, it improves performance to let the scheduler know what files in should process.


before explaining the next step, lets understand a key principle in airflow: the difference between dag processing and dag execution. airflow’s scheduler endlessly scans the dag files and creates dags, update them and check if dag can start running.
this is dag processing, and it is done inside the scheduler.
when a dag can be triggered, it starts a dag-run, and the scheduler triggers its execution in one of the available workers.


Hence, the next step was removing ALL network calls from the dag processing stage. Since airflow scans dags endlessly, any long workload might cause a dramatic performance change for all dags. we removed some calls to the scheduler db and to other external resources to the dag execution, where the dag really runs.


Last, but very not least (actually this was the most important change) we declared our imports in the python files in a very economical way. this means:
1. separating long files into smaller modules, where each file has its own imports.
2. when possible, using imports inside the function that uses it and not in the header of the file.


this datadog graph shows the change after the imports refactor.


since python is interpreted, when importing a file, python runs the file and all its dependencies recursively. this, along with the non-stop scheduler work requires a very strict python files load.
when investigating it a bit more we found out that open source packages were written with a call to airflow db in the ctor, which is a bad practice that we had to use it carefully, in a more lazy way.


more than that, in airflow documents it is not said loud enough how important is economical import strategy. I would say it is the first principle you need to adopt when developing on airflow. We found it on the hard way but I believe it could have been saved from us on earlier stage :)
