---
schema_version: "1.0.0"
document_id: "66c24ad0a1882460c08959db36178e93008a94954d4743eec30422948fd88ccd"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/streamline-document-processing-reducto-databricks"
published_at: "2025-06-26T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:2c0792665c2cc5a4e004923ddd8ae6a08c36bf215a8a04501b396fc697c01d6c"
---

# Streamlining Document Processing with Reducto and Databricks

Most enterprise data is locked inside unstructured formats—PDFs, scanned forms, and reports stored in object stores or databases, but not readily usable. Reducto exists to unlock that value. We help teams extract structured, meaningful information from documents at scale with state-of-the-art accuracy, powering everything from operational workflows to advanced ML pipelines.


With a flexible API and developer-friendly SDKs, Reducto transforms documents into structured outputs, whether you're extracting specific fields or generating full-text embeddings. Our goal is to make document ingestion feel as seamless and native as any other data source in your stack, including platforms like Databricks.


### Using Reducto on your data directly within Databricks


Your only prerequisite to get started is an API key and unstructured data in Databricks. You can get a free trial for Reducto. Just[contact us](http://reducto.ai/contact) and mention Databricks, and we’ll get you started.


The integration is simple: Take your documents housed in object storage (like S3 or Databricks volumes), within your tables, or referenced via pre-signed URLs and directly pass them into Reducto to extract specific fields or parse the full content.


From there, these outputs can be loaded into Spark dataframes and written directly into Delta Lake Tables. They’re now available everywhere in your Databricks ecosystem: queried with Databricks SQL, piped into ML pipelines (for LLMs, retrieval-augmented generation, feature stores, etc.), or visualized in BI tools like Tableau and Power BI.


### Walkthrough: A clean way to ingest documents at scale


To get the hang of using Reducto, we recommend starting with a small notebook that ingests a few files from your object store and appends them to a new Delta Table. This will get you up and running in minutes. From there, you can plug the results into downstream dashboards or feature pipelines.


This guide utilizes our[Python SDK](https://github.com/reductoai/reducto-python-sdk) in a Databricks notebook.


1. First, install Reducto. Make sure you’ve added your Reducto API key[to your secrets](https://docs.databricks.com/aws/en/security/secrets/) .


```text
python  !pip install reductoai
```


1. Import your data. You'll need to provide a path to your existing files:


```text
python  from pathlib import Path
from reducto import Reducto
import pandas as pd


api_key = dbutils.secrets.get(scope="reducto", key="REDUCTO_API_KEY")
client = Reducto(api_key=api_key)


folder = Path("/Workspace/Users/Reducto/blood_test_results")
records = []


for blood_test in folder.iterdir():
upload = client.upload(file=blood_test)


```


Or using binary data from a table:


```text
python  import tempfile, os
from pathlib import Path
from reducto import Reducto
import pandas as pd


api_key = dbutils.secrets.get(scope="reducto", key="REDUCTO_API_KEY")
client = Reducto(api_key=api_key)


records = []


spark_df = spark.read.table("lab_results.raw_files")
rows = spark_df.collect()


for row in rows:
file_name = row["file_name"]
bytes_ = row["contents"] # binary column


# Write bytes to a disposable temp file so Reducto can read it
suffix = Path(file_name).suffix or ".jpg"
with tempfile.NamedTemporaryFile(delete=False, suffix=suffix) as tmp:
tmp.write(bytes_)
tmp_path = Path(tmp.name)


# Everything here on is identical; upload and extract.
upload = client.upload(file=tmp_path)
```


1. Parse and extract your data


We’ll parse and extract the data together, first pulling the full data out of our documents and then extracting structured outputs out of the parse. You can call one or the other, but this example chains the parse output before running extract.


Here, we’ll:


- Parse the full content of bloodwork documents
- Extract specific fields including a patient’s DOB, and a few different blood sample data points


```text
python  parse_response = client.parse.run(
document_url=upload
options={
"ocr_mode": "standard",
"extraction_mode": "ocr",
"chunking": {"chunk_mode": "variable"},
"table_summary": {"enabled": False},
"figure_summary": {
"enabled": False,
"override": False
},
"force_url_result": False
}
)
# You can use this fully parsed response for something if needed!


job_id = parse_response.job_id


response = client.extract.run(
document_url=f"jobid://{job_id}",
system_prompt="Be precise and thorough. These are blood test results of varying page lengths and structures. Use visual layout cues such as bold labels, column alignment, and section dividers to interpret structure.",
options={
"ocr_mode": "standard",
"extraction_mode": "ocr",
"chunking": {"chunk_mode": "variable"},
"table_summary": {"enabled": False},
"figure_summary": {
"enabled": False,
"override": False
},
"force_url_result": False
},
schema={
"type": "object",
"properties": {
"patientName": {
"type": "string",
"description": "The full name of the patient."
},
"dateOfBirth": {
"type": "string",
"description": "The date of birth of the patient, formatted as YYYY-MM-DD."
},
"hemoglobinCount": {
"type": "number",
"description": "The hemoglobin count in the patient's blood, measured in grams per deciliter."
},
"redBloodCellCount": {
"type": "number",
"description": "The count of red blood cells in the patient's blood."
},
"whiteBloodCellCount": {
"type": "number",
"description": "The count of white blood cells in the patient's blood."
}
},
"required": [
"patientName",
"dateOfBirth",
"hemoglobinCount",
"redBloodCellCount",
"whiteBloodCellCount"
]
}
)
```


1. Write the extracted results


All that’s left is to write your data to your tables for your workflows to directly consume, or to use later.


```text
python  dataframe = pd.DataFrame(records)
spark_df = spark.createDataFrame(dataframe)


spark.sql("CREATE DATABASE IF NOT EXISTS lab_results")
(
spark_df
.write
.mode("append")
.saveAsTable("lab_results.blood_test_results")
)


print("Loaded", len(records), "records into lab_results.blood_test_results")
display(spark_df)


```


1. Use your parsed data


Your data is ready for use directly within Databricks. Common use cases include retrieval-augmented generation (RAG), generating LLM-ready inputs, and powering downstream applications, analytics, and business logic.


Our customers span industries such as legal, insurance, healthcare, and more, each unlocking insights from previously untapped document sources.


## Conclusion: Reducto as an Ingestion Layer


Integrating Reducto with Databricks gives teams a powerful way to unlock the value hidden in unstructured documents. Whether you’re dealing with medical records, contracts, invoices, or scanned forms, Reducto transforms them into structured, machine-readable data—ready for analytics, AI, or workflow automation.


We also have more Databricks integration examples in our[documentation here](https://docs.reducto.ai/recipes/databricks-integration) . You can find more documentation on parse, extract, and more[here](https://docs.reducto.ai/overview) . And if you want to see how Reducto works in an interactive playground, you can check it out[here](http://studio.reducto.ai/) as well. With a few lines of code, your PDFs and forms become structured, queryable, and ML-ready, living right alongside the rest of your data and processes.
