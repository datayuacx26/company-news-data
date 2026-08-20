---
schema_version: "1.0.0"
document_id: "1e7ac99678927016e97324ca7197169203b18913d639994f18a998ff0adde7bd"
company_key: "yc-oneschema"
company: "OneSchema"
source_id: "yc-oneschema-news-import-43da02420b1d"
canonical_url: "https://www.oneschema.co/blog/import-csv-bigquery"
published_at: null
first_seen_at: "2026-07-22T07:23:01.413777+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:68ed419c38d8222c212e70567978228b85b6679f7ed18f8457f735e9c33c6ca7"
---

# How to Import CSV into BigQuery

Google BigQuery is a serverless, highly scalable, and cost-effective multi-cloud data warehouse designed for big data.


In this article, we explore five methods to import CSV files into BigQuery, each suitable for different scenarios ranging from quick data loads to more complex, automated data pipelines.


‍


## **Method #1: BigQuery Web UI**


The BigQuery Web UI offers a user-friendly interface for importing CSV files directly into a BigQuery dataset. This method is straightforward and does not require writing code or using command-line tools.


### **Use case**


Ideal for manual, one-time imports where simplicity and ease of use are key.


### **Example usage**


1. Navigate to the BigQuery Web UI in the Google Cloud Console.
2. Select the dataset and choose to create a new table.
3. In the Create Table form, select "Upload" for the source, and then choose your CSV file.
4. Specify the table and column details, and then start the import.


‍


## **Method #2: bq Command-Line Tool**


The bq command-line tool is part of Google Cloud SDK and provides a way to interact with BigQuery from the command line. It's useful for scripting and automating data loads.


### **Use case**


Suitable for automated imports or when working within scriptable environments.


### **Example usage**


```text
bash
bq load --source_format=CSV your_dataset.your_table /path/to/your_file.csv /path/to/schema.json


```


**Note:** schema.json is a JSON file that defines the schema of the table.


‍


## **Method #3: Python Script with Google Cloud API**


Using Python along with the Google Cloud API provides a flexible and programmable way to import CSV files into BigQuery. This method is ideal for customizing the import process, including data transformation and preprocessing.


### **Use Case**


Perfect for scenarios requiring programmatic control over the CSV import process, such as data transformation, complex error handling, or integration into existing Python-based data pipelines.


### **Example Usage**


1. Set Up Your Environment:some text


1. Ensure you have Python installed along with the google-cloud-bigquery package. You can install it using pip:


```text
bash
pip install google-cloud-bigquery


```


1. Write a Python script to handle the CSV import. The script should authenticate with Google Cloud, read the CSV file, and then load the data into BigQuery.
2. Authenticate with Google Cloud:some text


1. Use service account credentials for authentication, ensuring you have the necessary permissions for BigQuery operations.


3. Read and Load the CSV File:some text


1. Use Python's built-in CSV handling capabilities or a library like Pandas to read the CSV file. The script should then use the BigQuery Client library to create a job that loads the CSV data into a BigQuery table. Optionally, add error handling and data transformation as needed.


**Example Python Script:**


```text
python
from google.cloud import bigquery
import csv


# Initialize a BigQuery client
client = bigquery.Client()


# Define your BigQuery dataset and table
dataset_id = 'your_dataset'
table_id = 'your_table'


# Path to your CSV file
filename = '/path/to/your_file.csv'


# Configure the load job
job_config = bigquery.LoadJobConfig(
source_format=bigquery.SourceFormat.CSV,
skip_leading_rows=1,  # Adjust if your CSV has a header
autodetect=True,      # Auto-detect schema
)


# Load the CSV data into BigQuery
with open(filename, "rb") as source_file:
job = client.load_table_from_file(source_file, f"{dataset_id}.{table_id}", job_config=job_config)


job.result()  # Wait for the job to complete


print(f"Loaded {job.output_rows} rows into {dataset_id}:{table_id}.")


```


‍


{{blog-content-cta}}


‍


## **Method #4: Streaming Inserts**


BigQuery allows for streaming data into a table, which means you can insert rows into a table one at a time. This method can be implemented using the BigQuery API in various programming languages, such as Python.


### **Use case**


Ideal for real-time data ingestion or when data is generated continuously.


### **Example usage**


```text
python
from google.cloud import bigquery


client = bigquery.Client()
table_id = 'your_project.your_dataset.your_table'


rows_to_insert = [
{"column1": "value1", "column2": "value2"}
]


errors = client.insert_rows_json(table_id, rows_to_insert)


```


‍


## **Method #5: Data Transfer Service**


BigQuery Data Transfer Service automates data movement into BigQuery on a scheduled, managed basis. It’s useful for recurring, automated data loads, like daily imports.


### **Use case**


Best for scheduled, recurring data imports from SaaS applications or other Google services.


### **Example usage**


1. Navigate to the BigQuery UI in Google Cloud Console.
2. Go to Transfers and create a new transfer.
3. Choose the data source and define the schedule and parameters for the data transfer.


‍


## **Conclusion**


Importing CSVs into Google BigQuery offers a range of methods, suitable for varying needs from simple, manual uploads to more sophisticated, automated data pipelines. The choice of method depends on factors like data size, frequency of import, and specific use cases.


If you’re looking for a comprehensive CSV import solution, consider OneSchema. OneSchema provides a powerful CSV parsing and importing tool that seamlessly integrates with your front-end framework of choice.
