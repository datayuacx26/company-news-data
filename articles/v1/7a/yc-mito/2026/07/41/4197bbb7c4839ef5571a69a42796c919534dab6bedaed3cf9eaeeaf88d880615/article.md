---
schema_version: "1.0.0"
document_id: "4197bbb7c4839ef5571a69a42796c919534dab6bedaed3cf9eaeeaf88d880615"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-connect-python-to-elasticsearch-complete-guide"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:5fe00a76408c5657683f8f40bd3d56829b77c258efa11fcdb74b3ba776da6de5"
---

# How to Connect Python to Elasticsearch: Complete Guide

[See more blogs](https://www.trymito.io/blog)


# How to Connect Python to Elasticsearch: Complete Guide


The Mito Team


10/6/2025


6 min read


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


Elasticsearch is a powerful distributed search and analytics engine built on Apache Lucene. When combined with Python, it provides exceptional full-text search, real-time analytics, and log analysis capabilities. This comprehensive guide covers everything you need to know about connecting Python to Elasticsearch.


## Why Use Elasticsearch with Python?


Elasticsearch with Python offers powerful capabilities:


- Lightning-fast full-text search across large datasets
- Real-time data indexing and searching
- Complex aggregations and analytics
- Distributed architecture for horizontal scaling
- Near real-time data availability
- RESTful API with comprehensive features
- Perfect for log analysis, search engines, and analytics
- Excellent support for geospatial queries


## Prerequisites


Before connecting Python to Elasticsearch, ensure you have:


- Python 3.6 or higher installed
- Elasticsearch installed and running (version 7.x or 8.x)
- Basic understanding of JSON and REST APIs
- Elasticsearch connection details (host, port, credentials)


## Installing the Elasticsearch Client


Install the official Elasticsearch Python client:


bash


```text
pip install elasticsearch
```


For Elasticsearch 8.x with newer features:


bash


```text
pip install elasticsearch>=8.0.0
```


## Basic Connection to Elasticsearch


Let's start with a simple connection:


python


```text
from elasticsearch import Elasticsearch


# Connect to local Elasticsearch
es = Elasticsearch(['http://localhost:9200'])


# Test connection
if es.ping():
print("Connected to Elasticsearch successfully!")
else:
print("Could not connect to Elasticsearch")


# Get cluster info
info = es.info()
print(f"Cluster name: {info['cluster_name']}")
print(f"Elasticsearch version: {info['version']['number']}")
```


## Connection with Authentication


For secured Elasticsearch instances:


python


```text
from elasticsearch import Elasticsearch


# Connect with basic authentication
es = Elasticsearch(
['https://localhost:9200'],
basic_auth=('username', 'password'),
verify_certs=True
)


print("Connected with authentication!")


# Alternative: API key authentication
es = Elasticsearch(
['https://localhost:9200'],
api_key=('id', 'api_key')
)
```


## Connection with Error Handling


python


```text
from elasticsearch import Elasticsearch
from elasticsearch.exceptions import ConnectionError, AuthenticationException


def create_es_connection():
try:
es = Elasticsearch(
['http://localhost:9200'],
request_timeout=30,
max_retries=3,
retry_on_timeout=True
)


if es.ping():
print("Successfully connected to Elasticsearch")


# Get cluster health
health = es.cluster.health()
print(f"Cluster status: {health['status']}")


return es
else:
print("Could not ping Elasticsearch")
return None


except ConnectionError as e:
print(f"Connection failed: {e}")
return None
except AuthenticationException as e:
print(f"Authentication failed: {e}")
return None
except Exception as e:
print(f"Error: {e}")
return None


# Usage
es = create_es_connection()
```


## Creating an Index


Indexes in Elasticsearch are like databases:


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Create index with settings
index_body = {
"settings": {
"number_of_shards": 1,
"number_of_replicas": 1
},
"mappings": {
"properties": {
"employee_id": {"type": "integer"},
"first_name": {"type": "text"},
"last_name": {"type": "text"},
"email": {"type": "keyword"},
"department": {"type": "keyword"},
"salary": {"type": "float"},
"hire_date": {"type": "date"},
"description": {"type": "text"}
}
}
}


# Create the index
es.indices.create(index='employees', body=index_body, ignore=400)


print("Index created successfully!")
```


## Indexing Documents


### Index Single Document


python


```text
from elasticsearch import Elasticsearch
from datetime import datetime


es = Elasticsearch(['http://localhost:9200'])


# Index a single document
doc = {
'employee_id': 1,
'first_name': 'John',
'last_name': 'Doe',
'email': 'john.doe@company.com',
'department': 'Engineering',
'salary': 75000.00,
'hire_date': datetime.now(),
'description': 'Senior software engineer with expertise in Python and distributed systems'
}


# Index with auto-generated ID
response = es.index(index='employees', document=doc)


print(f"Document indexed with ID: {response['_id']}")


# Index with specific ID
response = es.index(index='employees', id=1, document=doc)


print(f"Document indexed with ID: {response['_id']}")
```


### Bulk Indexing


For better performance with multiple documents:


python


```text
from elasticsearch import Elasticsearch
from elasticsearch.helpers import bulk


es = Elasticsearch(['http://localhost:9200'])


# Prepare bulk data
documents = [
{
'_index': 'employees',
'_id': 2,
'_source': {
'employee_id': 2,
'first_name': 'Jane',
'last_name': 'Smith',
'email': 'jane@company.com',
'department': 'Marketing',
'salary': 68000.00,
'description': 'Marketing specialist with 5 years of experience'
}
},
{
'_index': 'employees',
'_id': 3,
'_source': {
'employee_id': 3,
'first_name': 'Bob',
'last_name': 'Johnson',
'email': 'bob@company.com',
'department': 'Sales',
'salary': 72000.00,
'description': 'Sales executive focused on enterprise clients'
}
},
{
'_index': 'employees',
'_id': 4,
'_source': {
'employee_id': 4,
'first_name': 'Alice',
'last_name': 'Williams',
'email': 'alice@company.com',
'department': 'Engineering',
'salary': 80000.00,
'description': 'Full-stack developer specializing in React and Node.js'
}
}
]


# Bulk index
success, failed = bulk(es, documents)


print(f"Successfully indexed: {success} documents")
print(f"Failed: {failed} documents")
```


## Searching Documents


### Simple Search


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Search all documents
response = es.search(index='employees', body={"query": {"match_all": {}}})


print(f"Total hits: {response['hits']['total']['value']}\n")


for hit in response['hits']['hits']:
doc = hit['_source']
print(f"Name: {doc['first_name']} {doc['last_name']}, Dept: {doc['department']}")
```


### Match Query


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Search with match query
query = {
"query": {
"match": {
"description": "Python engineer"
}
}
}


response = es.search(index='employees', body=query)


print("Search Results:")
for hit in response['hits']['hits']:
doc = hit['_source']
score = hit['_score']
print(f"Score: {score} - {doc['first_name']} {doc['last_name']}")
print(f"Description: {doc['description']}\n")
```


### Term Query


For exact matches on keyword fields:


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Exact match on keyword field
query = {
"query": {
"term": {
"department": "Engineering"
}
}
}


response = es.search(index='employees', body=query)


print("Engineering Employees:")
for hit in response['hits']['hits']:
doc = hit['_source']
print(f"{doc['first_name']} {doc['last_name']} - ${doc['salary']}")
```


### Range Query


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Find employees with salary in range
query = {
"query": {
"range": {
"salary": {
"gte": 70000,
"lte": 80000
}
}
}
}


response = es.search(index='employees', body=query)


print("Employees with salary between $70k-$80k:")
for hit in response['hits']['hits']:
doc = hit['_source']
print(f"{doc['first_name']} {doc['last_name']} - ${doc['salary']}")
```


### Boolean Query


Combine multiple conditions:


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Complex boolean query
query = {
"query": {
"bool": {
"must": [
{"match": {"description": "engineer"}}
],
"filter": [
{"term": {"department": "Engineering"}},
{"range": {"salary": {"gte": 75000}}}
],
"should": [
{"match": {"description": "Python"}},
{"match": {"description": "Java"}}
],
"minimum_should_match": 1
}
}
}


response = es.search(index='employees', body=query)


print("Engineers matching criteria:")
for hit in response['hits']['hits']:
doc = hit['_source']
print(f"{doc['first_name']} {doc['last_name']} - ${doc['salary']}")
```


### Full-Text Search with Highlighting


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Search with highlighting
query = {
"query": {
"match": {
"description": "software engineer"
}
},
"highlight": {
"fields": {
"description": {}
}
}
}


response = es.search(index='employees', body=query)


for hit in response['hits']['hits']:
doc = hit['_source']
print(f"{doc['first_name']} {doc['last_name']}")


if 'highlight' in hit:
print(f"Highlighted: {hit['highlight']['description'][0]}\n")
```


## Updating Documents


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Update specific fields
update_body = {
"doc": {
"salary": 85000.00,
"department": "Senior Engineering"
}
}


response = es.update(index='employees', id=1, body=update_body)


print(f"Document updated: {response['result']}")


# Update with script
script_body = {
"script": {
"source": "ctx._source.salary += params.raise_amount",
"params": {
"raise_amount": 5000
}
}
}


response = es.update(index='employees', id=1, body=script_body)


print(f"Salary updated via script: {response['result']}")
```


## Deleting Documents


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Delete by ID
response = es.delete(index='employees', id=10)


print(f"Document deleted: {response['result']}")


# Delete by query
delete_query = {
"query": {
"term": {
"department": "Sales"
}
}
}


response = es.delete_by_query(index='employees', body=delete_query)


print(f"Deleted {response['deleted']} documents")
```


## Aggregations


Elasticsearch provides powerful aggregation capabilities:


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Statistics aggregation
agg_body = {
"size": 0,
"aggs": {
"by_department": {
"terms": {
"field": "department"
},
"aggs": {
"avg_salary": {
"avg": {"field": "salary"}
},
"max_salary": {
"max": {"field": "salary"}
},
"min_salary": {
"min": {"field": "salary"}
}
}
},
"overall_stats": {
"stats": {
"field": "salary"
}
}
}
}


response = es.search(index='employees', body=agg_body)


# Process aggregation results
print("Department Statistics:")
for bucket in response['aggregations']['by_department']['buckets']:
dept = bucket['key']
count = bucket['doc_count']
avg_sal = bucket['avg_salary']['value']
max_sal = bucket['max_salary']['value']
min_sal = bucket['min_salary']['value']


print(f"\n{dept}:")
print(f"  Employees: {count}")
print(f"  Avg Salary: ${avg_sal:.2f}")
print(f"  Salary Range: ${min_sal:.2f} - ${max_sal:.2f}")


# Overall statistics
stats = response['aggregations']['overall_stats']
print(f"\nOverall Statistics:")
print(f"  Total Employees: {stats['count']}")
print(f"  Average Salary: ${stats['avg']:.2f}")
```


## Pagination


python


```text
from elasticsearch import Elasticsearch


es = Elasticsearch(['http://localhost:9200'])


# Pagination using from and size
page_size = 10
page_number = 0


query = {
"query": {"match_all": {}},
"from": page_number * page_size,
"size": page_size,
"sort": [
{"last_name.keyword": "asc"}
]
}


response = es.search(index='employees', body=query)


print(f"Page {page_number + 1}:")
for hit in response['hits']['hits']:
doc = hit['_source']
print(f"{doc['last_name']}, {doc['first_name']}")


# Scroll API for large result sets
query = {
"query": {"match_all": {}},
"size": 100
}


# Initial scroll request
response = es.search(index='employees', body=query, scroll='2m')


scroll_id = response['_scroll_id']
hits = response['hits']['hits']


# Continue scrolling
while len(hits) > 0:
# Process current batch
for hit in hits:
doc = hit['_source']
print(f"{doc['first_name']} {doc['last_name']}")


# Get next batch
response = es.scroll(scroll_id=scroll_id, scroll='2m')
scroll_id = response['_scroll_id']
hits = response['hits']['hits']


# Clear scroll
es.clear_scroll(scroll_id=scroll_id)
```


## Complete Application Example


python


```text
from elasticsearch import Elasticsearch
from elasticsearch.helpers import bulk
from datetime import datetime


class EmployeeSearch:
def __init__(self, host='localhost', port=9200):
self.es = Elasticsearch([f'http://{host}:{port}'])
self.index_name = 'employees'
self.create_index()


def create_index(self):
if not self.es.indices.exists(index=self.index_name):
mapping = {
"settings": {
"number_of_shards": 1,
"number_of_replicas": 1
},
"mappings": {
"properties": {
"employee_id": {"type": "integer"},
"first_name": {"type": "text"},
"last_name": {"type": "text"},
"email": {"type": "keyword"},
"department": {"type": "keyword"},
"salary": {"type": "float"},
"hire_date": {"type": "date"},
"skills": {"type": "keyword"},
"description": {"type": "text"}
}
}
}
self.es.indices.create(index=self.index_name, body=mapping)
print(f"Index '{self.index_name}' created")


def add_employee(self, employee_data):
response = self.es.index(
index=self.index_name,
document=employee_data
)
return response['_id']


def search_by_name(self, name):
query = {
"query": {
"multi_match": {
"query": name,
"fields": ["first_name", "last_name"]
}
}
}


response = self.es.search(index=self.index_name, body=query)
return [hit['_source'] for hit in response['hits']['hits']]


def search_by_department(self, department):
query = {
"query": {
"term": {
"department": department
}
}
}


response = self.es.search(index=self.index_name, body=query)
return [hit['_source'] for hit in response['hits']['hits']]


def search_by_skills(self, skills):
query = {
"query": {
"terms": {
"skills": skills
}
}
}


response = self.es.search(index=self.index_name, body=query)
return [hit['_source'] for hit in response['hits']['hits']]


def full_text_search(self, text):
query = {
"query": {
"match": {
"description": {
"query": text,
"fuzziness": "AUTO"
}
}
}
}


response = self.es.search(index=self.index_name, body=query)
return [hit['_source'] for hit in response['hits']['hits']]


def get_department_stats(self):
agg_body = {
"size": 0,
"aggs": {
"departments": {
"terms": {"field": "department"},
"aggs": {
"avg_salary": {"avg": {"field": "salary"}}
}
}
}
}


response = self.es.search(index=self.index_name, body=agg_body)
return response['aggregations']['departments']['buckets']


# Usage
search_engine = EmployeeSearch()


# Add employees
emp_id = search_engine.add_employee({
'employee_id': 1,
'first_name': 'Sarah',
'last_name': 'Connor',
'email': 'sarah@company.com',
'department': 'Security',
'salary': 78000,
'hire_date': datetime.now(),
'skills': ['Security', 'Risk Management', 'Compliance'],
'description': 'Security specialist with expertise in threat assessment'
})


print(f"Employee added with ID: {emp_id}")


# Search by name
results = search_engine.search_by_name('Sarah')
print(f"Found {len(results)} employees named Sarah")


# Department stats
stats = search_engine.get_department_stats()
for bucket in stats:
print(f"{bucket['key']}: {bucket['doc_count']} employees, "
f"Avg Salary: ${bucket['avg_salary']['value']:.2f}")
```


## Best Practices


Follow these best practices when using Elasticsearch with Python:


1. **Use bulk operations** for indexing multiple documents
2. **Define appropriate mappings** before indexing data
3. **Use keyword type** for exact match


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
