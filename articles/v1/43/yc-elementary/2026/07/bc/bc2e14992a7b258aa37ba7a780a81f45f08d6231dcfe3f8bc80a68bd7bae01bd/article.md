---
schema_version: "1.0.0"
document_id: "bc2e14992a7b258aa37ba7a780a81f45f08d6231dcfe3f8bc80a68bd7bae01bd"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/data-quality-in-python-pipelines"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:71d47a9fafe9841fb7b37cd8dcb47453d32a0ad57e91cbc6d53427fed5943d28"
---

# Data Quality in Python Pipelines: Introducing the Elementary Python SDK | Elementary Data

More teams are shifting their data quality checks out of dashboards and into the transformation layer itself. It’s obvious why: the transformation code is the first place that touches real data. If a check fails here, the pipeline stops before corrupted rows ever land downstream. No backfills, no detective work, no “how long has this been wrong?” scramble.


And the value cuts both ways:


- **You catch issues before they ever hit the data warehouse or lake** , right at the ingestion and preprocessing layers.
- **You catch issues after the data warehouse too** , in the pipelines that stream data to downstream destinations, models, APIs, and operational systems.


Python has also become the backbone of modern data engineering, especially in pipelines that go beyond SQL. It now drives:


- Ingestion and storage of unstructured data
- Vectorization and embedding pipelines for AI systems
- ML model training and feature generation
- Monitoring of model inputs and outputs
- Hybrid pipelines that mix structured, semi-structured, and free-form data


As these pipelines multiply, Python becomes the glue. It runs wherever data flows, before the DWH, inside the DWH, and after the DWH, making it the natural place for data quality and observability to live.


## Wrapping Existing Tools Instead of Inventing New Ones


Engineers already have strong opinions about how they want to write tests. Some rely on Great Expectations, others on DQX, or homegrown frameworks. Reinventing a new test engine or DSL would just fragment the landscape.


So we didn’t.


We focused on the simplest possible layer: **a lightweight Python SDK that captures any Python test result, from any framework** , and reports it to Elementary. You keep your code, we handle the metadata, structure, and visibility.


This means full observability without dictating how you build.


## Built for Teams That Treat Their Data Pipelines Like Software


Elementary has always leaned into engineering-first workflows. Our deep integration with dbt set that foundation. Extending this into Python is the natural continuation of that approach.


As more transformations shift into Python (Pyspark, SQL generation, AI/ML pipelines, unstructured data processing), teams want the same capabilities they rely on when using Elementary with dbt:


- Understand what ran
- Track when it ran
- Measure how long it took
- Identify which upstream assets fed it
- Trace which downstream assets it produced
- Run data quality checks on the product and see the results
- Get alerts on data issues as soon as they happen


The SDK provides exactly that by wrapping the transformation code itself. You get execution metadata, lineage, run context, and full test surface, directly from inside your existing codebase.


This unifies **Analytics Engineering, Data Science, and AI/ML Operations** into a single observability platform. Python + dbt + cloud tests now all land in one place.


## Announcing Assets and Test Results with the Python SDK


This example shows how to define and run Python data quality tests on a sample DataFrame, attach them to an Elementary asset, and automatically send the test results to Elementary Cloud.


```text
# define "unique ids" test
@boolean_test(
name=  "unique_ids"  ,
description=  "All user IDs must be unique"  ,
column_name=  "id"  ,
)
def     test_unique_ids  (  df: pd.DataFrame  ) ->   bool  :
ids = df[  "id"  ].dropna().tolist()
return     len  (ids) ==   len  (  set  (ids))


# define "average age" test
@expected_range(
name=  "average_age"  ,
min  =  18  ,
max  =  50  ,
description=  "Average age should be between 18 and 50"  ,
column_name=  "age"  ,
)
def     test_average_age  (  df: pd.DataFrame  ) ->   float  :
return   df[  "age"  ].mean()


def     main  ():
# Create sample data
users_df = pd.DataFrame(
{
"id"  : [  1  ,   2  ,   3  ,   4  ,   5  ,   6  ,   7  ,   8  ],
"age"  : [  23  ,   30  ,   46  ,   76  ,   76  ,   123  ,   45  ,   32  ],
"country"  : [  "Germany"  ,   "France"  ,   "Germany"  ,   "France"  ,   ""  ,   "Italy"  ,   "France"  ,   "Germany"  ],
}
)
# Define the tested asset
asset = TableAsset(
name=  "users"  ,
database_name=  "prod"  ,
schema_name=  "public"  ,
table_name=  "users"  ,
description=  "Users table"  ,
owners=[  "data-team"  ],
tags=[  "pii"  ,   "production"  ],
depends_on=[  "prod.public.customers"  ,   "prod.public.orders"  ]
)
with   elementary_test_context(asset=asset)   as   ctx:
# run tests and report the results
test_average_age(users_df)
test_unique_ids(users_df)
client = ElementaryCloudClient(PROJECT_ID, TOKEN, URL)
client.send_to_cloud(ctx)
```


‍


‍` ‍` This is how the tests look in the Python pipeline logs:


## What You’ll See in Elementary Once You Report Through the SDK


When a Python pipeline reports assets, test results, and execution metadata, everything shows up in Elementary unified with your dbt and cloud tests:


**The test will be available in the test overview screen, which includes** the test execution history:


**The test query:**


**And the test configuration:**


**All test results appear together** - Python validations, dbt tests, cloud tests, all in a single, consistent interface.


‍


**Alerts fire through your existing channels** (Slack, PagerDuty, email), ensuring that pipeline-level issues trigger the same operational flow as warehouse-level ones.


‍


**Incidents are created automatically** for detected issues, including opening Jira tickets. Elementary’s agentic tools then investigate root cause, assess downstream impact, and guide resolution.


‍


**Lineage becomes fully connected** , tying together Python assets, dbt models, warehouse tables, unstructured data, vectors, and ML outputs.


‍


**Every table, view, file, or vector store entity produced by Python becomes discoverable** through the Elementary catalog, data discovery agent, and MCP server — giving analysts, DS, and AI teams a shared understanding of the entire data ecosystem.


This closes the gap between ingestion pipelines, warehouse transformations, ML prep code, and AI workloads — all observed in one place.


‍


## Now in Beta


The SDK is now in beta and already surfacing surprisingly rich insights from Python-based workflows. If you want early access or want to see how it fits your implementation, reach out — we’re shaping this with real teams using it at scale.


## Closing Thoughts


Python is now the default environment for AI-era data pipelines. That means observability needs to meet engineers where they already work. By capturing every test, every asset, and every run, no matter the framework, this SDK connects your ingestion pipelines, dbt models, ML workflows, and AI systems into a single shared platform.


If you want to see your existing tests show up in Elementary with minimal effort, or explore how the platform visualizes Python-driven assets, we’d love to talk.


‍
