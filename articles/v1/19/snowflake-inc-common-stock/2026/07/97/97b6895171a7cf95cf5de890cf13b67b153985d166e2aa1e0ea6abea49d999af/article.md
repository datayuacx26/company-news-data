---
schema_version: "1.0.0"
document_id: "97b6895171a7cf95cf5de890cf13b67b153985d166e2aa1e0ea6abea49d999af"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/migrate-spark-to-snowflake"
published_at: "2026-07-30T23:55:00+00:00"
first_seen_at: "2026-07-31T17:44:58.928283+00:00"
fetched_at: "2026-07-31T17:45:00.742064+00:00"
content_hash: "sha256:ac8c43184e62cf4192f21a4df0a0fce3c06be8553364c7b83c101dca85aae542"
---

# Run Apache Spark on Snowflake with CoCo

Running Spark is not just about writing transformations and business logic. It also means tuning clusters, patching infrastructure and managing dependency environments.


Moving those workloads onto Snowflake addresses that directly, with customers experiencing up to 5.1x faster performance and 42% lower costs1.[Snowpark Connect for Apache Spark™](https://www.snowflake.com/en/product/features/snowpark-connect-apache-spark/) lets your existing Spark code run on Snowflake's engine with minimal changes; no clusters to provision, tune or patch.


Most generic AI coding assistants can fall short when it comes to migration at scale. They can rewrite a snippet, but they lack the compatibility context to move an entire codebase to a new engine: which patterns are unsupported, how to map them to DataFrame equivalents and how to record what changed. The` spark-migration` skill in[Snowflake CoCo](https://www.snowflake.com/en/product/snowflake-coco/) , a data-native AI coding agent, is built to close that gap.


## One prompt, one codebase


The clearest way to see the skill is to watch it run. The demo below starts with a directory of data pipelines written in PySpark.


From there, the prompt is simple. Just ask CoCo to migrate a file or directory of files to Snowpark Connect. This will launch the` spark-migration` skill coordinating the` snowpark-connect` migration path that will guide you through making your Spark code compatible with Snowpark Connect. Note that the input file(s) for this skill could be Python, Scala or Java code files (as well as the build files). The skill can also take in notebook files to ensure they are compatible with Snowflake.


*The demo shows a large Spark codebase checked for compatibility and migrated to Snowflake from a single prompt.*


*Figure 1: A directory of PySpark pipelines scanned, rewritten and reported on from one conversational prompt.*


## How to migrate PySpark to Snowflake with CoCo


Behind that single prompt, CoCo's agentic workflow runs through the steps a migration expert would:


1. **It does an assessment and builds an inventory:** This skill scans every source file for compatibility issues including RDD operations, certain UDF serialization patterns and unsupported file formats, referencing a knowledge base built by Snowflake's engineering team. Each file, dependency and unique API call are inventoried.
2. **It applies targeted fixes:** The core conversion is done by dispatching parallel agents to apply code fixes and verifying the results through multiple gates, including syntax compilation and evidence-based checks. During this phase, incompatible patterns are rewritten to DataFrame equivalents, imports and session creation are updated and anything that needs manual review (such as validating reads and writes) is flagged with a detailed explanation.
3. **It generates predictable reporting that can be consumed by people or agents:** This gives you full visibility into every change made and what may still need attention by generating issue logs, inventory reports and a validation of the overall migration state.
4. **It validates the converted code's functionality:** The validation phase performs an end-to-end validation by running the original PySpark code and the migrated Snowpark Connect code against the same synthetic data and comparing their outputs table-by-table. This catches behavioral differences and validates the code is ready to run. Any fixes discovered during validation are automatically committed back to the deliverable branch, so the result is both a pass/fail verdict and a more complete migration. This validation phase is optional.


Instead of searching across files for incompatible patterns and cross-referencing documentation, the developer stays in one workflow while CoCo handles the scanning, fixing and reporting needed to get Spark code running on Snowflake.


## More than a single file


Migration is rarely one clean step, so the skill meets a codebase wherever it is. A few other ways to point it at your work:


- **Scope the effort before you start:** *"Assess the compatibility and level of effort required to migrate this codebase."*
- **Validate before you run:** *"Validate that this codebase will run with Snowpark Connect."*
- **Complete another tool's migration:** *"Analyze the output of any other migration tool and complete the migration."*


The skill auto-activates when you mention Spark, PySpark or code migration; or you can invoke it directly with` spark-migration` . As noted before, it will flag anything that needs manual review, so you stay in control of what changes.


## Try it yourself


That is a compelling offer: less manual compatibility testing, less refactoring and a faster path to running Spark workloads natively on Snowflake. What used to take hours of manual effort, even with existing deterministic tools, now takes minutes.


The` spark-migration` skill is bundled with CoCo; no setup required.


Try it: *"Migrate this file to Snowpark Connect."*


Get started with CoCo and Spark migration by referring to[this documentation](https://docs.snowflake.com/en/migrations/sma-docs/migrating-with-cortex-code/README) .


1 Based on customer production use cases and proof-of-concept exercises comparing the speed and cost for Snowpark versus managed Spark services between November 2022 and May 2025. All findings summarize actual customer outcomes with real data and do not represent fabricated datasets used for benchmarks.
