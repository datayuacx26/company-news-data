---
schema_version: "1.0.0"
document_id: "1d6af2063ec9414f56f23478384bfbc30bc22b377f5a0bdbee00a42f308df15f"
company_key: "tripadvisor-inc-common-stock"
company: "TripAdvisor Inc."
source_id: "tripadvisor-inc-common-stock-rss-6295d6870799"
canonical_url: "https://medium.com/tripadvisor/level-up-experimentation-scaling-custom-metrics-at-viator-6287e324e872"
published_at: "2026-03-23T15:35:58+00:00"
first_seen_at: "2026-07-20T23:18:15.449539+00:00"
fetched_at: "2026-08-20T00:33:41.838119+00:00"
content_hash: "sha256:17d99865dbecbb47cc7c43adbea940503e6714b9696e206f4c6720620249f0ce"
---

# Level Up Experimentation: Scaling Custom Metrics at Viator

Photo credit:Andrii Yalanskyi / Shutterstop


*How Viator built a scalable data stack with dbt, Airflow, PySpark, and EMR.*


By[José Morais](https://moraisjose.com/) , Software Data Engineer


At Viator, we’re all about experimentation. The faster we can experiment and gather insights, the better decisions we can make. That’s why we embarked on a mission to build a robust framework for custom metric calculation. Building this custom metric framework wasn’t only about increasing experimentation speed; the initiative aimed to empower engineers and product managers to define new data-driven metrics and gain deeper, actionable insights across the Viator product analytics ecosystem.


### Legacy data workflow challenges in experimentation platform


Before our new framework, calculating custom metrics was a challenging task. Our previous setup relied heavily on Airflow DAGs and SQL queries, resulting in a rigid, unscalable process. We’d process daily actions, now supplanted by dbt-generated fact tables, perform hard-coded aggregations, then trigger a Jupyter Notebook for statistical analysis.


Our previous data pipeline approach created challenges for scalability and agility:


- **Scalability** : The Jupyter Notebook at the end of the pipeline proved to be a significant bottleneck, especially as the number of experiments increased.
- **Backfill headaches** : Fixing data for even a single experiment meant reprocessing massive amounts of data, a time-consuming, costly ordeal.
- **Poor modularity** : Adding new metrics was incredibly difficult and cumbersome due to the hard-coded nature of the SQL queries.


We were constantly battling a lack of scalability and the inability to quickly create new metrics to analyze experiments effectively. It was more complex and costly to fix data issues, significantly slowing our feedback loops.


### Modern experimentation architecture using PySpark and BigQuery


Our new architecture is a game-changer, addressing the core pain points of the previous system. While we leveraged dbt for building foundational fact tables, which contain crucial experiment and variant information, the real muscle of our new system lies in a powerful PySpark job running on an EMR Serverless.


Here’s how the custom metric calculation framework works:


**1.** **Fact table foundation (dbt) for custom metrics** : We utilize dbt to create our fact tables in BigQuery. The initial use case focuses on user-based events from various sources, including apps, websites, and bookings. The framework now supports any type of event and offers high flexibility, so it handles user-centric, page-centric, or other events easily. The framework’s flexibility easily enables future use cases such as SEO experimentation and SEM campaign analytics. The key is the integration with our experimentation platform, Vela, which is responsible for allocating events and associating entities (like user_id, page_url, or locale) with specific experiments and variants through its mapping capabilities.


**2. Spark’s heavy lifting (EMR Serverless):** The daily, and potentially more frequent, Spark job, triggered by Airflow DAGs, generated dynamically by Vela based on fact table information, orchestrates the entire metric calculation process.


- It streams all active experiments with their associated metrics.
- Then it processes experiments in parallel batches. For each experiment, it queries BigQuery to aggregate data related to all of its associated metrics and perform statistical analysis.
- Finally, it writes the aggregated DataFrames back to BigQuery, overwriting the previous results for each experiment.


**3.** **Intelligent data handling** : We actively cache aggregated DataFrames for further usage (writing to BigQuery and statistical analysis) and unpersist them after each experiment run. We also partition entity aggregation DataFrames by entity and variant aggregation DataFrames by variant, optimizing performance.


### Collaborative framework and user empowerment


One of the key strengths of this new framework is its modularity and collaborative design. Our project structure follows engineering best practices, featuring clear separation between core components, configuration, utilities, and an analytics package owned and managed by the analytics team. The modular architecture equips our analytics team to easily add their logic for metric aggregations and statistical analysis. They maintain the query builders for entity and variant aggregations, along with the logic for statistical analysis.


We integrated with Vela UI to put control directly into the hands of our users:


- **Fact table registration** : Users can select a fact table from BigQuery, and the UI lists its columns, allowing them to map these to entities, facts, and timestamps.
- **Metric definition** : Users can specify the fact (table column), aggregation type (sum, count, average, etc.), and filters to define new metrics.
- **Experiment configuration** : At the experiment level, users can associate metrics and define custom properties such as minimum detectable effect (MDE) and winsorization.


Vela’s metadata-driven configurations guarantee consistent custom metric calculations across diverse user events and data sources.


### Simplified and cost-efficient data backfilling


One of the most significant wins with our new architecture is the ease of backfilling data. In the past, correcting data for even a single experiment meant reprocessing enormous datasets, a costly and time-consuming endeavor. Now, thanks to the modularity of our Spark jobs and the flexibility of Airflow, backfills are simpler.


We leverage the Airflow API to trigger specific DAGs from Vela which enables us to pass override parameters that define precisely which experiments need reprocessing. Instead of rerunning the entire pipeline for all experiments, we can target individual experiments, drastically reducing the time and computational resources required. Granular DAG-level control in Airflow supports faster fixes, more efficient resource utilization, and quicker iterations on our experimentation data.


### Observability, monitoring, and detection for Spark jobs


Beyond processing data, a critical component of our new architecture is its robust observability. We implemented comprehensive monitoring using Grafana dashboards that provide real-time insights into every layer of our system.


Comprehensive observability covers Airflow DAG statuses, Spark interval performance, and real-time data quality monitoring. We detect unusual spikes or anomalies in the processed data to forecast potential issues, allowing us to act proactively before major incidents escalate.


Our alert system detects these deviations and notifies our teams, enabling them to respond swiftly and minimize any impact. Furthermore, the Spark job prioritizes resilience: when an experiment calculation fails, the system processes the next one, so no single failure derails the entire workflow. All errors are captured and made visible, enabling precise fixes and targeted backfills without affecting ongoing operations. By integrating proactive workflow design, our data pipeline consistently delivers reliable A/B test results and high-quality experimentation insights.


### Performance results and the future of our framework


The effect of this new architecture has been significant:


- **Performance at scale** : We can now process hundreds of experiments, each involving hundreds of GB to TB of event data over its lifetime.
- **Developer velocity and onboarding** : The ease of the backfill process and the modular design have significantly boosted developer velocity.
- **Custom metrics and backfill from UI** : This infrastructure has unblocked our analytics and data science teams, enabling them to define and manage custom metrics with ease and initiate backfills directly from the UI.


### Success powered by cross-function collaboration


Viator Experimentation Platform team enjoys a bright day in the London office.Photo credit: Viator


This ambitious project wouldn’t have been possible without the incredible dedication and collaborative spirit of multiple teams and individuals. A massive thank-you to everyone who contributed to shaping and building this framework:


- **Analytics engineering** : A special shout-out to[Artur Trindade](https://www.linkedin.com/in/arturtrindade/) for his pivotal role in building the dbt models that form the backbone of our fact tables.
- **Software engineering** : Our deep gratitude to[João Franco](https://www.linkedin.com/in/joao-c-franco/) ,[Rui Menoita](https://www.linkedin.com/in/ruimenoita/) ,[Bartosz Malcherek](https://www.linkedin.com/in/bartosz-malcherek/) , and[Aleksander Kowalczyk](https://www.linkedin.com/in/aleksander-kowalczyk-5b39aa204/) for their expertise in crafting the robust UI and core platform components.
- **Data science and analytics** : Thank you to[Tatia Engelmore](https://www.linkedin.com/in/tatia-engelmore-14490911/) ,[Marc Jaffee](https://www.linkedin.com/in/marc-jaffee/) ,[Haider Tari](https://www.linkedin.com/in/haider-tari-690096387/) , and and[Dan Pritchard](https://www.linkedin.com/in/dan-pritchard-a90922147/) for their invaluable contributions to metric calculation logic and statistical analysis.
- **Product** : To[Felipe Vieira](https://www.linkedin.com/in/felipe-vieira-b45a8526/) for his vision and guidance, ensuring our framework directly addresses business needs and empowers our users.


The success of scaling custom metrics for experimentation at Viator was a cross-functional effort, showcasing the power of working together to solve complex problems and deliver significant business value.


*Interested in working with us? View our*[open positions](https://careers.tripadvisor.com/) *today!*


---


[Level Up Experimentation: Scaling Custom Metrics at Viator](https://medium.com/tripadvisor/level-up-experimentation-scaling-custom-metrics-at-viator-6287e324e872) was originally published in[Tripadvisor Tech](https://medium.com/tripadvisor) on Medium, where people are continuing the conversation by highlighting and responding to this story.
